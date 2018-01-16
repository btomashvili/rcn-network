### NANO LOAN ENGINE
* createLoan(oracleContract, borrower, cosigner, cosignerFee, currncy, amount, interestRate, interestRatePunitory, duesIn, cancelableAt, expirationRequest) 

Creates a loan request, the loan can be generated with any borrower, cosigner, and conditions; if the cosigner and borrower agree they must call the "approve" function.
The creator of the loan is the caller of this function; this is useful to track which wallet created the loan.

* approve() Called by the members of the loan to show that they agree with the terms of the loan; the borrower and the cosigner must call this method before any lender could call the method "lend".



* isApproved(uint index) Return true if the loan has been approved by the borrower and cosigner


- Other Nano loan engine helper methods 
* getLoanConfig()
* getLoanState()
* getTotalLoans()
* getOracle()
* getCosigner()
* getLender()
* getCreator()
* getAmount()




#### COSIGNER METHODS

* Creates a liability to pay a loan if the borrower defaults.
* Destroys a previously created liability, also destroys the associated loan on the engine
* Withdraw tokens from the smart contract.
* Transfering the ownership of the smart contract
* Withdraw funds from a loan
* Transfers a loan to a new owner
* Claims the coverage of a defaulted loan
        The lender owning the defaulted loan must call this method to claim his compensation; before it must
        call "approveTransfer" on the engine, passing the index of the defaulted loan and the address of the cosigner.

        When this method is called, the loan is transferred from the lender to this contract, and the tokens
        corresponding to the compensation are transferred from this contract to the lender, this contract has to have
        enough tokens to pay to the lender.

        RETURNS : true if the compensation was claimed

* Defines a custom logic that determines if a loan is defaulted or not.  
    RETURNS : true if the loan is considered defaulted)

* Retrieves the rate of the loan's currency in RCN, provided by the oracle;
  RETURNS:  Equivalent of the currency in RCN


* Retrieves the currency parameter of a loan
  RETURNS: currency parameter of a loan, in string format


#### ORACLE METHODS

* Sells the current rate of a given symbol, the msg.sender must previously call "approve" on the RCN Token
     with the cost defined in the method "getCost", throws if not implemented.
     RETURNS: The current rate of the symbol

* Provides the current rate of a symbol without charging the msg.sender, cannot be  used by contracts  it's intended use is to know in advance how much RCN will take an operation using this oracle.  
RETURNS : The current rate of a symbol

* IsContract() Returns true if an address is an ethereum contract
* The cost of calling "getRate" could be fixed in RCN or using the same currency of the oracle, it also can 
 RETURNS: The current cost of calling "getRate" for the same symbol, if it returns 0 the call is free.

*  getDecimals(string symbol) RETURNS The unit used to define the rate of the currency
* createSymbol(......) Adds a symbol to the oracle, Unit used to define the rate of the currency, Ex: 2 = cents
* setRate(symbolm newRate) Updates the rate of a symbol and the timestamp of the last update.
* setCost(symbol, newCost) Changes the cost of calling "getRate", keeping the costInCurrency setting. 
* transfer(address) Transfers the ownership of the oracle
* setProvider(address) Sets a provider, the provider is an address that can update the cost and rate of the oracle symbols.
* setBlackListed(address, bool blocklisted) Blacklisted address cannot call "getRate" method; this is a security feature added to avoid "proxy" oracles


