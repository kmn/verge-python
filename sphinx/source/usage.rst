=================
 Usage
=================

See also the `main verge documentation`_ for details and background on setting up and
using verged remotely.

Setting up verge for remote control
-------------------------------------

If you run VERGE with the ``-server`` argument, or if you run ``verged``, it can be controlled 
either by sending it HTTP-JSON-RPC commands.

However, beginning with VERGE 0.3.3 you must create a ``VERGE.conf`` file in the VERGE data directory 
(default ``$HOME/.vergeconf``) and set an RPC password:

::

  rpcuser=anything
  rpcpassword=anything

Once that is done, the easiest way to check whether VERGE accepts remote commands is by running 
VERGE again, with the command (and any parameters) as arguments. For example:

::

  $ verged getinfo

Connecting to the wallet from Python
-------------------------------------

There are two functions for this:

*Connecting to local verge instance*
  Use the function :func:`~vergerpc.connect_to_local`. This automagically
  sorts out the connection to a verge process running on the current machine,
  for the current user.
  
  ::
  
    conn = vergerpc.connect_to_local()

*Connecting to a remote verge instance*
  Use the function :func:`~vergerpc.connect_to_remote`. For this function
  it is neccesary to explicitly specify a hostname and port to connect to, and
  to provide user credentials for logging in.

  ::
  
    conn = vergerpc.connect_to_remote('foo', 'bar', host='payments.yoyodyne.com', port=20102)


How to use the API
-------------------------------------

For basic sending and receiving of payments, the four most important methods are 

*Getting the current balance*
  Use the method :func:`~vergerpc.connection.VERGEConnection.getbalance` to get the current server balance.
  
  ::
  
    print "Your balance is %f" % (conn.getbalance(),)

*Check a customer address for validity and get information about it*
  This can be done with the method :func:`~vergerpc.connection.VERGEConnection.validateaddress`.

  ::

      rv = conn.validateaddress(foo)
      if rv.isvalid:
          print "The address that you provided is valid"
      else:
          print "The address that you provided is invalid, please correct"

*Sending payments*
  The method :func:`~vergerpc.connection.VERGEConnection.sendtoaddress` sends a specified
  amount of coins to a specified address.

  ::

      conn.sendtoaddress("msTGAm1ApjEJfsWfAaRVaZHRm26mv5GL73", 10000.0)

*Get a new address for accepting payments*
  To accept payments, use the method :func:`~vergerpc.connection.VERGEConnection.getnewaddress`
  to generate a new address. Give this address to the customer and store it in a safe place, to be able to check
  when the payment to this address has been made.

  ::
  
      pay_to = conn.getnewaddress()
      print "We will ship the pirate sandwidth after payment of 200 coins to ", pay_to

*Check how much has been received at a certain address*
  The method :func:`~vergerpc.connection.VERGEConnection.getreceivedbyaddress` 
  returns how many verges have been received at a certain address. Together with the
  previous function, this can be used to check whether a payment has been made
  by the customer.

  ::

      amount = conn.getreceivedbyaddress(pay_to)
      if amount > 20000.0:
          print "Thanks, your order will be prepared and shipped."



      
The account API
-------------------------------------
More advanced usage of verge allows multiple accounts within one wallet. This
can be useful if you are writing software for a bank, or 
simply want to have a clear separation between customers payments.

For this, see the `Account API`_ documentation.

.. _main bitcoin documentation: https://en.bitcoin.it/wiki/Main_Page
.. _account API: https://en.bitcoin.it/wiki/Accounts_explained


