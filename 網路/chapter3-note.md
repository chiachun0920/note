# 傳輸層與網路層之關係
|  網路世界  | 現實生活  |
|---------- |-----------|
|應用程式訊息|信封中的文字 |
|行程|房子內的人|
|主機|房子|
|傳輸層協定|房子內負責寄信/收信的人|
|網路層協定|郵差|

# 可靠資料傳輸的原理
- rdt 1.0：假設底層通道是可靠的
```javascript
    //sender
    class ApplicationLayer{

        public send(){

            TrnasportLayer.rdtSend(data);            
        }

    }
    class TransportLayer{
        
        public rdtSend(data){
            let packet = makePacket(data);
            NetworkLayer.udtSend(packet);
        }
    }
```
```js
    //receiver
    class TransportLayer{

        update(){
            Network.rdtReceive().subscribe((packet)=>{
                let data = extract(packet);
                ApplicationLayer.deliverData(data);
            })    
        }
    }
```
- rdt 2.0：會產生位元錯誤的通道
    - 致命缺點
        - 接收端並不會知道自己送出的確認有沒有毀損
```js
    //sender
    class TransportLayer{
        
        public s1:State = WaitUpperLayerCalledState();
        public s2:State = WaitAckOrNakState();
        public state;

        constructor(){
            this.state = s1;
        }

        rdtSend(data){
            
            this.state.send(data);
        }
    }
    class WaitUpperLayerCalledState{

        send(data){
            
            transportLayer.sendPacket = makePacket(data,checksum);
            NetworkLayer.udtSend(sendPacket);
            transportLayer.state = transportLayer.s2;
        }
    }
    class WaitAckOrNakState(){

        constructore(){
            Network.redReceive().subscribe((receivePacket)=>{

                if(isAck(receivePakcet)){
                    transportLayer.state = transportLayer.s1;
                }
                else if(isNak(receivePakcet)){
                    NetworkLayer.udtSend(transportLayer.sendPacket);
                }
            })
        }
    }
```
```js
    //receiver
     class TransportLayer{
        
        public s1:State = WaitLowerLayerCalledState();
        
        public state;

        constructor(){
            this.state = s1;
        }
    }
    class WaitLowerLayerCalledState{        

        constructor(){

            NetworkLayer.rdtRecive().subscribe((packet)=>{

                if(isCorrupt(packet)){

                    let sendPacket = makePacket('NAK');
                    NetworkLayer.udtSend(sendPacket);
                }else {
                    
                    let data = extract(packet);
                    ApplicationLayer.deliverData(data);

                    let sendPacket = makePacket('ACK');
                    NetworkLayer.udtSend(sendPacket);
                }
            })

        }
    }
```
- rdt 2.1
    - 因應rdt 2.0在面對毀損的NAK或ACK時候，會重新送出封包，讓接收端無法判對現在是新的封包還是重複的封包
    - 加入**序號**概念
    - 接收端只需要檢查序號就可以知道收到的封包是否為重送的封包
```js
    //sender
    class TransportLayer{
        
        public s1:State = WaitUpperLayerCalled0State();
        public s2:State = WaitAckOrNak0State();
        public s3:State = WaitUpperLayerCalled1State();
        public s4:State = WaitAckOrNak1State();
        public state;

        constructor(){
            this.state = s1;
        }

        rdtSend(data){
            
            this.state.send(data);
        }
    }
    class WaitUpperLayerCalled0State{

        send(data){
            
            transportLayer.sendPacket = makePacket(0,data,checksum);
            NetworkLayer.udtSend(sendPacket);
            transportLayer.state = transportLayer.s2;
        }
    }
    class WaitAckOrNak0State(){

        constructore(){
            Network.rdtReceive().subscribe((receivePacket)=>{

                if(isAck(receivePakcet)&&!isCorrupt(receivePacket)){
                    transportLayer.state = transportLayer.s3;
                }
                else if(isNak(receivePakcet)||isCorrupt(receivePacket)){
                    NetworkLayer.udtSend(transportLayer.sendPacket);
                }
            })
        }
    }
    class WaitUpperLayerCalled1State{

        send(data){
            
            transportLayer.sendPacket = makePacket(1,data,checksum);
            NetworkLayer.udtSend(sendPacket);
            transportLayer.state = transportLayer.s4;
        }
    }
    class WaitAckOrNak1State(){

        constructore(){
            Network.rdtReceive().subscribe((receivePacket)=>{

                if(isAck(receivePakcet)&&!isCorrupt(receivePacket)){
                    transportLayer.state = transportLayer.s1;
                }
                else if(isNak(receivePakcet)||isCorrupt(receivePacket)){
                    NetworkLayer.udtSend(transportLayer.sendPacket);
                }
            })
        }
    }
```
