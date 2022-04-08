<MainRs>

```rust
use near_sdk::{env, near_bindgen, AccountId};
use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};

use models::{Donation, STORAGE_COST}

near_sdk::setup_alloc!();

#[near_bindgen]
#[derive(Default, BorshDeserialize, BorshSerialize)]
pub struct DonationTracker {
    beneficiary: AccountId,
    donations: PersistentVector<Donation>
}

#[near_bindgen]
impl DonationTracker {

  pub fn init(&self, beneficiary: AccountId) -> Self {
      DonationTracker::new(beneficiary)
  }

  pub fn donate(&mut self): i32 {
      // Get who is calling the method, and how much NEAR they attached
      let donator: AccountId = env::predecessor();
      let amount: Balance = env::attachedDeposit();

      // Send almost all of it to the beneficiary (deduct some to cover storage)
      Contract(self.beneficiary).transfer(amount - STORAGE_COST);
    
      // Record the donation
      const donation_number: i32 = self.add_donation(donator, amount);
      let log_message = format!("Thank you {}, your donation is the number {}", donator, donation_number);
      env::log(log_message.as_bytes());

      return donation_number
  }

  fn add_donation(&mut self): i32{
      let donation: Donation = Donation::new(from, amount);
      self.donations.append(donation);
      return self.donations.length
  }

  pub fn get_donation_by_number(&self, donation_number: i32): Donation {
    assert!(donation_number > 0 &&  donation_number <= self.donations.length, "Invalid donation number")
    return self.donations[donation_number - 1] 
  }
}
```

</MainRs>