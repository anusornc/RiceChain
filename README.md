# RiceChain
Secure and Traceable Rice Supply Chain Framework Using Blockchain Technology
pragma solidity ^0.4.26;

contract RiceChain1 {
    address public owner; // 
   struct Stakeholders{ 
     address farmer;// address of the famer
      address RGE; 
      address RGP; 
      address RGD; 
      address RGR; 
      address Consumer;
      address SSC;
        //address TracingGateway;
   }
    mapping (address=> Stakeholders) holders;
    
   
    //mapping (address=> farmProducts[]) products;
   
  
    
    //struct sscMap{ 
       // address SSC;// address of the SSC
    //    address farmer; 
    //}
     //mapping (address=> sscMap) SSCMap;
       //event addedHoldersMapping( address famer, address SSC, address RGE, address RGP, address RGD, address RGR, address Consumer);
    
     string SSCStatus;
     string FarmerStatus;
     string Contract_Status;
     string prize;

       //return FarmerStatus="SupplyRequested";
       //return SSCStatus="read";
    
     
     struct SeedPurchase{
        int quantity; // user seed quantity
        string SType; // seed type
        string SBrand;//seed brand
        int SPrize; // seed prize
        string  FarmerStatus; // status of the famer
        address farmer;
        address SSC;
        string SSCStatus;
        string Contract_Status;
    }
    
     //SeedPurchase [] public seedPurchasing; 
     mapping (address => SeedPurchase) public seedPurchasing;
      mapping (address => address[]) public FarmProducts;
      
       function addFarmerSSCMapping(address SSC, address farmer) public { 
        //only admin can add serviceZone and homes
        FarmProducts[SSC].push(farmer);
        emit FarmerSSCMappingAdded(SSC,farmer,msg.sender);
    }   event FarmerSSCMappingAdded(address farmer,address SSC,address Owner);
    
    function BuySeedFromSSC(address farmer, address SSC, int SPrize, string memory SBrand, int quantity, string memory SType ) public
    returns (string memory) {
    bool farmerExists=false;
    //FarmerStatus="SupplyRequested";
    //SSCStatus="read";
    //Contract_Status="Created!";
    {
        for(uint256 i = 0; i<FarmProducts[SSC].length; i++){
            if(FarmProducts[SSC][i]==farmer){ // check if farmer exist
                farmerExists=true;
               //return "Farmer Authenticated!";
               emit FarmerAuthenticated(farmer,SSC,msg.sender);
                break;
            }
        }
                
           if (farmerExists){ 
                 
                if (SPrize==1){ //Sprize must be 0 or 1. 0 is used if payment not made, and 1 is used if payment is made. 
                    
                    prize="Payed";
                    //prize="Payed";
                    Contract_Status="Request On Process";
                    FarmerStatus="Service Wait";
                    SSCStatus="Sales Approved";
                   //return "Farmer_Authenticated";
                   seedPurchasing[farmer].quantity=quantity;
                    seedPurchasing[farmer].SType=SType;
                    seedPurchasing[farmer].SBrand=SBrand;
                    seedPurchasing[farmer].SPrize=SPrize;
                    seedPurchasing[farmer].farmer=farmer;
                    seedPurchasing[farmer].SSC=SSC;
                    seedPurchasing[farmer].FarmerStatus=FarmerStatus;
                    seedPurchasing[farmer].SSCStatus=SSCStatus;
                    seedPurchasing[farmer].Contract_Status=Contract_Status;
                  //return "Farmer Authenticated & Payment Successful!";
                  emit SeedBuy_Success_Notification(SSCStatus, FarmerStatus, SSC , farmer, SBrand, quantity, SType, prize);
                
            }
            else{
                //prize="Not Payed";
                SSCStatus="Sales Not Approved";
                FarmerStatus="Service Declined";
                Contract_Status="Request Declined!";
                    seedPurchasing[farmer].quantity=quantity;
                    seedPurchasing[farmer].SType=SType;
                    seedPurchasing[farmer].SBrand=SBrand;
                    seedPurchasing[farmer].SPrize=SPrize;
                    seedPurchasing[farmer].farmer=farmer;
                    seedPurchasing[farmer].SSC=SSC;
                    seedPurchasing[farmer].FarmerStatus=FarmerStatus;
                    seedPurchasing[farmer].SSCStatus=SSCStatus;
                    seedPurchasing[farmer].Contract_Status=Contract_Status;
                //trigger PaymentNotMade event
                //return "Farmer Authenticated but Payment Not Made!";
                emit PaymentNotMade(Contract_Status, SSCStatus, FarmerStatus, farmer, SSC, msg.sender);
            }
           }
    else{
        
        
        //trigger FarmerDoesnotExist event
            emit FarmerDoesnotExist(farmer,SSC,msg.sender);
            // trigger failed authentication event
            emit NotAuthenticated(msg.sender);
            }
        
        }
       
    }
        event SeedBuy_Success_Notification(string SSCStatus, string FarmerStatus, address sellingCompany, address Farmer, string Seed_Brand, int Quantity, string Seed_Type, string Seed_Prize);
        event FarmerAuthenticated(address farmer,address SSC,address Authenticator);
        event PaymentNotMade(string Contract_Status, string SSCStatus, string FarmerStatus,address Farmer,address SSC, address Checker);
        event FarmerDoesnotExist(address Farmer,address SSC, address Authenticator);
        event NotAuthenticated(address Authenticator);
   
    //string SBrand; 
    //int quantity; 
   // string SType;     
    function getFarmerDetails(address farmer, address SSC) public  view returns ( string memory,
    string memory, string memory, string memory, int) {
      bool farmerAvailable =false;
      for(uint256 m = 0; m<FarmProducts[SSC].length; m++){
            if(FarmProducts[SSC][m]==farmer){ // check if farmer exist
                farmerAvailable=true;  
        break;
       }
      }
        if (farmerAvailable){ 
                 
                if (seedPurchasing[farmer].SPrize==1){
                 return (prize, FarmerStatus, SSCStatus, Contract_Status, seedPurchasing[farmer].quantity);  
                }
       else{
           prize="Not Payed";
           SSCStatus="Sales Not Approved";
           FarmerStatus="Service Declined";
           Contract_Status="Request Declined!";
           return (prize, FarmerStatus, SSCStatus, Contract_Status, seedPurchasing[farmer].quantity);
         }
        }    
    }
}

//##########################################################################################################################################
//##########################################################################################################################################

pragma solidity ^0.4.26;

contract RiceChain1 {
    address public owner; // 
 
    
   
    //mapping (address=> farmProducts[]) products;
   
  
    
    //struct RGEMap{ 
       // address RGE;// address of the RGE
    //    address RGP; 
    //}
     
     string RGEStatus;
     string RGPStatus;
     string Contract_Status;
     string prize;
    
     
     struct BuyingGrain{
        int Quantity; // user seed Quantity
        string DatePurchased; // seed type
        //string SBrand;//seed brand
        int GPrize; // seed prize
        string  RGPStatus; // status of the famer
        address RGP;
        address RGE;
        string RGEStatus;
        string Contract_Status;
    }
    
     //SeedPurchase [] public grainBuy; 
     mapping (address => BuyingGrain) public grainBuy;
      mapping (address => address[]) public GrainProducts;
      
       function addRGP_RGEMapping(address RGE, address RGP) public { 
        //only admin can add serviceZone and homes
        GrainProducts[RGE].push(RGP);
        emit RGP_RGEMappingAdded(RGP,RGE,msg.sender);
    }   event RGP_RGEMappingAdded(address RGE,address RGP,address Owner);
    
    function BuyGrainFromRGP(address RGE, address RGP, int GPrize, string memory DatePurchased, int Quantity) public
    returns (string memory) {
    bool RGPExists=false;
    RGPStatus="SupplyRequested";
    RGEStatus="read";
    Contract_Status="Created!";
    {
        for(uint256 i = 0; i<GrainProducts[RGE].length; i++){
            if(GrainProducts[RGE][i]==RGP){ // check if RGP exist
                RGPExists=true;
               
               emit RGPAuthenticated(RGP,RGE,msg.sender);
                break;
            }
        }
                
           if (RGPExists){ 
                 
                if (GPrize==1){ //GPrize must be 0 or 1. 0 is used if payment not made, and 1 is used if payment is made. 
                    
                    prize="Payed";
                    //prize="Payed";
                    Contract_Status="Request Approved!";
                    RGPStatus="Wait for grain";
                    RGEStatus="Grain released successful!";
                   //return "RGP_Authenticated";
                   grainBuy[RGP].Quantity=Quantity;
                    grainBuy[RGP].DatePurchased=DatePurchased;
                    //grainBuy[RGP].SBrand=SBrand;
                    grainBuy[RGP].GPrize=GPrize;
                    grainBuy[RGP].RGP=RGP;
                    grainBuy[RGP].RGE=RGE;
                    grainBuy[RGP].RGPStatus=RGPStatus;
                    grainBuy[RGP].RGEStatus=RGEStatus;
                    grainBuy[RGP].Contract_Status=Contract_Status;
                   //return "RGP Authenticated & Payment Successful!";
                   emit BuyGrain_Success_Notification(RGEStatus, RGPStatus, RGE , RGP, Quantity, DatePurchased, prize);
                
            }
            else{
                prize="Not Payed";
                RGEStatus="Grain released failed!";
                RGPStatus="Request not Successful!";
                Contract_Status="Request Denied!";
                    grainBuy[RGP].Quantity=Quantity;
                    grainBuy[RGP].DatePurchased=DatePurchased;
                    //grainBuy[RGP].SBrand=SBrand;
                    grainBuy[RGP].GPrize=GPrize;
                    grainBuy[RGP].RGP=RGP;
                    grainBuy[RGP].RGE=RGE;
                    grainBuy[RGP].RGPStatus=RGPStatus;
                    grainBuy[RGP].RGEStatus=RGEStatus;
                    grainBuy[RGP].Contract_Status=Contract_Status;
                //trigger PaymentNotMade event
                //return "REQUEST DENIED!: RGP Authenticated but Payment Not Made!";
                emit PaymentNotMade(Contract_Status, RGPStatus, RGEStatus, RGP, RGE, prize, msg.sender);
            }
           }
    else{
        
        
        //trigger RGPDoesnotExist event
            emit RGPDoesnotExist(RGP,RGE,msg.sender);
            // trigger failed authentication event
            emit NotAuthenticated(msg.sender);
            }
        
        }
       
    }
        event BuyGrain_Success_Notification(string RGEStatus, string RGPStatus, address sellingCompany, address RGP, int Quantity, string DatePurchased, string Seed_Prize);
        event RGPAuthenticated(address RGP,address RGE,address Authenticator);
        event PaymentNotMade(string Contract_Status, string RGPStatus, string RGEStatus,address RGP,address RGE, string prize, address Checker);
        event RGPDoesnotExist(address RGP,address RGE, address Authenticator);
        event NotAuthenticated(address Authenticator);
   
    //string SBrand; 
    //int Quantity; 
   // string DatePurchased;     
    function getRGPDetails(address RGP, address RGE) public  view returns ( string memory,
    string memory, string memory, string memory, int) {
      bool RGPAvailable =false;
      for(uint256 m = 0; m<GrainProducts[RGE].length; m++){
            if(GrainProducts[RGE][m]==RGP){ // check if RGP exist
                RGPAvailable=true;  
        break;
       }
      }
        if (RGPAvailable){ 
                 
                if (grainBuy[RGP].GPrize==1){
                 return (prize, RGPStatus, RGEStatus, Contract_Status, grainBuy[RGP].Quantity);  
                }
       else{
           prize="Not Payed";
           RGEStatus="Grain released failed!";
           Contract_Status="Request_Declined!";
           return (prize, RGPStatus, RGEStatus, Contract_Status, grainBuy[RGP].Quantity);
         }
        }    
    }
}

//##########################################################################################################################################
//##########################################################################################################################################

pragma solidity ^0.4.26;

contract RiceChain3 {
    address public owner; // 
 
    
   
    //mapping (address=> farmProducts[]) products;
   
  
    
    //struct RGDMap{ 
       // address RGD;// address of the RGD
    //    address RGR; 
    //}
     
     string RGDStatus;
     string RGRStatus;
     string Contract_Status;
     string Product_prize;
     string Product_Sales;
    
     
     struct SellingToRGR{
        int Quantity; // user seed Quantity
        string DatePurchased; // seed type
        //string SBrand;//seed brand
        int ProductPayment; // seed Product_prize
        string  RGRStatus; // status of the famer
        address RGR;
        address RGD;
        string RGDStatus;
        string Contract_Status;
    }
    
     //SeedPurchase [] public RGRbuy; 
     mapping (address => SellingToRGR) public RGRbuy;
      mapping (address => address[]) public RiceProducts;
      
       function addRGR_RGDMapping(address RGD, address RGR) public { 
        //only admin can add serviceZone and homes
        RiceProducts[RGD].push(RGR);
        emit RGR_RGDMappingAdded(RGR,RGD,msg.sender);
    }   event RGR_RGDMappingAdded(address RGD,address RGR,address Owner);
    
    function SellGrainToRGR(address RGD, address RGR, int ProductPayment, string memory DatePurchased, int Quantity) public
    returns (string memory) {
    bool RGRExists=false;
    //RGRStatus="Prepared To Buy";
    //RGDStatus="RGP Product Received";
    //Contract_Status="RGD Sales";
    {
        for(uint256 i = 0; i<RiceProducts[RGD].length; i++){
            if(RiceProducts[RGD][i]==RGR){ // check if RGR exist
                RGRExists=true;
               
                break;
            }
        }
                
           if (RGRExists){ 
                 
                if (ProductPayment==1){ //ProductPayment  must be 0 or 1. 0 is used if payment not made, and 1 is used if payment is made. 
                    
                    Product_prize="Payed";
                    Product_Sales="Accepted";
                    //Product_prize="Payed";
                    Contract_Status="Product Buyin Successful!";
                    RGRStatus="Product Delivery Successful";
                    RGDStatus="Product Sold TO RGR!";
                   //return "RGR_Authenticated";
                   RGRbuy[RGR].Quantity=Quantity;
                    RGRbuy[RGR].DatePurchased=DatePurchased;
                    //RGRbuy[RGR].SBrand=SBrand;
                    RGRbuy[RGR].ProductPayment=ProductPayment;
                    RGRbuy[RGR].RGR=RGR;
                    RGRbuy[RGR].RGD=RGD;
                    RGRbuy[RGR].RGRStatus=RGRStatus;
                    RGRbuy[RGR].RGDStatus=RGDStatus;
                    RGRbuy[RGR].Contract_Status=Contract_Status;
                  // return "RGR Authenticated, Payment Successful and Product sell Accepted!";
                    emit RGRAuthenticated(RGR,RGD,msg.sender);
                    emit SellGrain_Success_Notification(RGDStatus, RGRStatus, RGD , RGR, Quantity, DatePurchased, Product_prize);
                
            }
            else{
                Product_prize="Not Payed";
                Product_Sales="Not-Accepted";
                RGDStatus="Product Request Failed!";
                RGRStatus="Product Delivery Failed!";
                Contract_Status="Product Buying Denied!";
                //trigger PaymentNotMade event
                    RGRbuy[RGR].Quantity=Quantity;
                    RGRbuy[RGR].DatePurchased=DatePurchased;
                    //RGRbuy[RGR].SBrand=SBrand;
                    RGRbuy[RGR].ProductPayment=ProductPayment;
                    RGRbuy[RGR].RGR=RGR;
                    RGRbuy[RGR].RGD=RGD;
                    RGRbuy[RGR].RGRStatus=RGRStatus;
                    RGRbuy[RGR].RGDStatus=RGDStatus;
                    RGRbuy[RGR].Contract_Status=Contract_Status;
                //return "REQUEST DENIED!: RGR Authenticated but Payment Not Made!";
                emit PaymentNotMade(Contract_Status, RGDStatus, RGRStatus, RGR, RGD, Product_Sales, msg.sender);
            }
           }
    else{
        
        
        //trigger RGRDoesnotExist event
            emit RGRDoesnotExist(RGR,RGD,msg.sender);
            // trigger failed authentication event
            emit NotAuthenticated(msg.sender);
            }
        
        }
       
    }
        event SellGrain_Success_Notification(string RGDStatus, string RGRStatus, address sellingCompany, address RGR, int Quantity, string DatePurchased, string Seed_Product_prize);
        event RGRAuthenticated(address RGR,address RGD,address Authenticator);
        event PaymentNotMade(string Contract_Status, string RGDStatus, string RGRStatus,address RGR,address RGD, string Product_Sales, address Checker);
        event RGRDoesnotExist(address RGR,address RGD, address Authenticator);
        event NotAuthenticated(address Authenticator);
   
    //string SBrand; 
    //int Quantity; 
   // string DatePurchased;     
    function getRGRDetails(address RGR, address RGD) public  view returns ( string memory, string memory,
    string memory, string memory, int) {
      bool RGRAvailable =false;
      for(uint256 m = 0; m<RiceProducts[RGD].length; m++){
            if(RiceProducts[RGD][m]==RGR){ // check if RGR exist
                RGRAvailable=true;  
        break;
       }
      }
        if (RGRAvailable){ 
                 
                if (RGRbuy[RGR].ProductPayment==1){
                 return (Product_prize, Product_Sales, RGRStatus, RGDStatus, RGRbuy[RGR].Quantity);  
                }
       else{
           Product_prize="Not Payed";
           Product_Sales="Not-Accepted";
           RGDStatus="Product Request Failed!";
           RGRStatus="Product Delivery Failed!";
           //Contract_Status="Request_Declined!";
           return (Product_prize, Product_Sales, RGRStatus, RGDStatus, RGRbuy[RGR].Quantity);
         }
        }    
    }
}

//##########################################################################################################################################
//##########################################################################################################################################

pragma solidity ^0.4.26;

contract RiceChain4 {
    address public owner; // 
 
    
   
    //mapping (address=> farmProducts[]) products;
   
  
    
    //struct RGDMap{ 
       // address RGD;// address of the RGD
    //    address RGR; 
    //}
     
     string ConsumerStatus;
     string RGRStatus;
     string Contract_Status;
     string Payment_Status;
     //string Product_Sales;
    
     
     struct SellingToConsumer{
        int Quantity; // user seed Quantity
        string DatePurchased; // seed type
        //string SBrand;//seed brand
        int ProductPayment; // seed Payment_Status
        string  RGRStatus; // status of the famer
        address RGR;
        address RGD;
        address Consumer;
        string ConsumerStatus;
        string Contract_Status;
        bytes32 Product_ID;
        bytes32 Sell_ID;
    }
    
     //SeedPurchase [] public ConsumerBuy; 
     mapping (address => SellingToConsumer) public ConsumerBuy;
      mapping (address => address[]) public RiceProducts;
      
       function addRGR_RGDMapping(address RGD, address RGR) public { 
        //only admin can add serviceZone and homes
        RiceProducts[RGD].push(RGR);
        emit RGR_RGDMappingAdded(RGR,RGD,msg.sender);
    }   event RGR_RGDMappingAdded(address RGD,address RGR,address Owner);
    
    function SellRiceToConsumer(address RGD, address RGR, address Consumer, int ProductPayment, string memory DatePurchased, int Quantity) public
    returns (string memory) {
    bool RGRExists=false;
    //RGRStatus="Prepared To Buy";
    //ConsumerStatus="RGP Product Received";
    //Contract_Status="RGD Sales";
    {
        for(uint256 i = 0; i<RiceProducts[RGD].length; i++){
            if(RiceProducts[RGD][i]==RGR){ // check if RGR exist
                RGRExists=true;
               
                break;
            }
        }
                
           if (RGRExists){ 
                 
                if (ProductPayment==1){ //ProductPayment  must be 0 or 1. 0 is used if payment not made, and 1 is used if payment is made. 
                    
                    Payment_Status="Payed";
                    //Product_Sales="Accepted";
                    //Payment_Status="Payed";
                    Contract_Status="Sold To Consumer!";
                    RGRStatus="Sales Successful";
                    ConsumerStatus="Purchase Successful!";
                    bytes32 Product_ID= keccak256(abi.encodePacked(RGD,RGR,msg.sender,block.timestamp));
                    bytes32 Sell_ID=keccak256(abi.encodePacked(RGR,Consumer,msg.sender,block.timestamp));
                   //return "RGR_Authenticated";
                    ConsumerBuy[RGR].Quantity=Quantity;
                    ConsumerBuy[RGR].DatePurchased=DatePurchased;
                    ConsumerBuy[RGR].ProductPayment=ProductPayment;
                    ConsumerBuy[RGR].RGR=RGR;
                    ConsumerBuy[RGR].RGD=RGD;
                    ConsumerBuy[RGR].RGRStatus=RGRStatus;
                    ConsumerBuy[RGR].ConsumerStatus=ConsumerStatus;
                    ConsumerBuy[RGR].Contract_Status=Contract_Status;
                    ConsumerBuy[RGR].Product_ID=Product_ID;
                    ConsumerBuy[RGR].Sell_ID=Sell_ID;
                    ConsumerBuy[RGR].Consumer=Consumer;
                   //return "RGR Authenticated, Payment Successful and Product sell Complited!";
                   emit RGRAuthenticated(RGR,RGD,msg.sender);
                  emit Consumer_Success_Notification(Contract_Status, ConsumerStatus, RGRStatus, RGD , RGR, Quantity, Payment_Status, Sell_ID, Product_ID);
                
            }
            else{
                Payment_Status="Not Payed";
                //Product_Sales="Not-Accepted";
                ConsumerStatus="Purchase Failed!";
                RGRStatus="Sales Failure!";
                Contract_Status="Sale Request Denied!";
                //trigger PaymentNotMade event
                
                    ConsumerBuy[RGR].Quantity=Quantity;
                    ConsumerBuy[RGR].DatePurchased=DatePurchased;
                    ConsumerBuy[RGR].ProductPayment=ProductPayment;
                    ConsumerBuy[RGR].RGR=RGR;
                    ConsumerBuy[RGR].RGD=RGD;
                    ConsumerBuy[RGR].RGRStatus=RGRStatus;
                    ConsumerBuy[RGR].ConsumerStatus=ConsumerStatus;
                    ConsumerBuy[RGR].Contract_Status=Contract_Status;
                    ConsumerBuy[RGR].Product_ID=Product_ID;
                    ConsumerBuy[RGR].Consumer=Consumer;
                    //ConsumerBuy[RGR].Sell_ID=Sell_ID;
                //return "REQUEST DENIED!: RGR Authenticated but Payment Not Made!";
                emit PaymentNotMade(Contract_Status, Payment_Status, RGRStatus, Consumer, RGR, Product_ID, msg.sender);
            }
           }
    else{
        
        
        //trigger RGRDoesnotExist event
            emit RGRDoesnotExist(RGR,RGD,msg.sender);
            // trigger failed authentication event
            emit NotAuthenticated(msg.sender);
            }
        
        }
       
    }
    event Consumer_Success_Notification(string Contract_Status, string ConsumerStatus, string RGRStatus, address Distributor, address Retailer, int Quantity, string Seed_Payment_Status, bytes32 Sell_ID, bytes32  Product_ID);
        event RGRAuthenticated(address RGR,address RGD,address Authenticator);
        event PaymentNotMade(string ontract_Status, string Payment_Status, string RGRStatus, address Consumer, address Retailer, bytes32 Product_ID, address Checker);
        event RGRDoesnotExist(address RGR,address RGD, address Authenticator); 
        event NotAuthenticated(address Authenticator);
   
    //string SBrand; 
    //int Quantity; 
   // string DatePurchased;     
    function getPurchaseDetails(address RGR, address RGD) public  view returns ( string memory, bytes32, string memory, 
     string memory, int) {
      bool RGRAvailable =false;
      for(uint256 m = 0; m<RiceProducts[RGD].length; m++){
            if(RiceProducts[RGD][m]==RGR){ // check if RGR exist
                RGRAvailable=true;  
        break;
       }
      }
        if (RGRAvailable){ 
                 
                if (ConsumerBuy[RGR].ProductPayment==1){
                 return (Payment_Status,ConsumerBuy[RGR].Sell_ID, RGRStatus, ConsumerStatus, ConsumerBuy[RGR].Quantity);  
                }
       else{
           Payment_Status="Not Payed";
           //Product_Sales="Not-Accepted";
           ConsumerStatus="Product Request Failed!";
           RGRStatus="Product Delivery Failed!";
           //Contract_Status="Request_Declined!";
           
           return (Payment_Status, ConsumerBuy[RGR].Product_ID, RGRStatus, ConsumerStatus, ConsumerBuy[RGR].Quantity);
         }
        }    
    }
}


