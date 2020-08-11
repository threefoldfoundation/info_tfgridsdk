### Reserve IT capacity by creating a capacity pool

To be able to deploy workloads on the TFGrid, a user needs to first have reserved some IT capacity from a farmer. To do so, a user creates a pool of capacity which contains a certain amount of [cloud units](https://wiki.threefold.io/#/cloud_units). 
For each workloads deployed, a certain amount of cloud units will be deducted from the pool as long as the workloads is alive.

Here is an example code snippet on how to create a capacity pool and pay for it:

```python
# get a reference to the 0-OS SAL
zos = j.sals.zos

# create a capacity pool with 50 compute units and 50 storage units on the farm called "farm_name"
# currencies contains a list of currency you are willing to use to pay for the capacity.
# this function return an object container the detail of the payment to be made to reserve the capacity
payment_detail = zos.pools.create(cu=50, su=50, farm="farm_name", currencies=["TFT", "FreeTFT"])

# get a reference to your TFT wallet
wallet = j.clients.stellar.get('my_wallet')

# proceed to the payment of the capacity
# this function will create and send a transaction to the explorer
# it returns the hash of the stellar transaction created
txs = zos.billing.payout_farmers(wallet, payment_detail)

# once the transaction is sent, it can take up to 2 minutes to see the pool populated with the cloud units.

# get the pool detail
pool = zos.pools.get(payment_detail.reservation_id)
print("compute units available:", pool.cus)
print("storage units available:", pool.cus)
```