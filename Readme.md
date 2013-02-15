Explicit soft deletion for ActiveRecord via deleted_at + callbacks and optional default scope.<br/>
Not overwriting destroy or delete.

Install
=======
    gem install soft_deletion

Usage
=====
    require 'soft_deletion'

    class User < ActiveRecord::Base
      has_soft_deletion :default_scope => true

      before_soft_delete :validate_deletability # soft_delete stops if this returns false
      after_soft_delete :send_deletion_emails

      has_many :products
    end

    # soft delete them including all dependencies that are marked as :destroy, :delete_all, :nullify
    user = User.first
    user.products.count == 10
    user.soft_delete!
    user.deleted? # true

    # use special with_deleted scope to find them ...
    user.reload # ActiveRecord::RecordNotFound
    User.with_deleted do
      user.reload # there it is ...
      user.products.count == 0
    end

    # soft undelete them all
    user.soft_undelete!
    user.products.count == 10

    # soft delete many
    User.soft_delete_all!(1,2,3,4)

Authors
=======

### [Contributors](https://github.com/grosser/soft_deletion/contributors)
 - [Michel Pigassou](https://github.com/Dagnan)
 - [Steven Davidovitz](https://github.com/steved555)
 - [PikachuEXE](https://github.com/PikachuEXE)
 - [Noel Dellofano](https://github.com/pinkvelociraptor)

[Zendesk](http://zendesk.com)<br/>
michael@grosser.it<br/>
License: MIT<br/>
[![Build Status](https://travis-ci.org/grosser/soft_deletion.png)](https://travis-ci.org/grosser/soft_deletion)
