<?php
/**
 * Magento
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Open Software License (OSL 3.0)
 * that is bundled with this package in the file LICENSE.txt.
 * It is also available through the world-wide-web at this URL:
 * http://opensource.org/licenses/osl-3.0.php
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@magentocommerce.com so we can send you a copy immediately.
 *
 * DISCLAIMER
 *
 * Do not edit or add to this file if you wish to upgrade Magento to newer
 * versions in the future. If you wish to customize Magento for your
 * needs please refer to http://www.magentocommerce.com for more information.
 *
 * @category    Mage
 * @package     Mage_Newsletter
 * @copyright   Copyright (c) 2013 Magento Inc. (http://www.magentocommerce.com)
 * @license     http://opensource.org/licenses/osl-3.0.php  Open Software License (OSL 3.0)
 */


/**
 * Newsletter subscriber resource model
 *
 * @category    Mage
 * @package     Mage_Newsletter
 * @author      Magento Core Team <core@magentocommerce.com>
 */
class Newsletter_Newsletter_Model_RW_Newsletter_Resource_Subscriber extends Mage_Newsletter_Model_Mysql4_Subscriber
{
    /**
     * Load subscriber from DB by email
     *
     * @param string $subscriberEmail
     * @return array
     */
    public function loadByEmail($subscriberEmail)
    {
    		
        /** @var $customerSession Mage_Customer_Model_Session */
        $customerSession = Mage::getSingleton('customer/session');
        $ownerId = Mage::getModel('customer/customer')
            ->setWebsiteId(Mage::app()->getStore()->getWebsiteId())
            ->loadByEmail($subscriberEmail)
            ->getId();
 
        $storeId = $customerSession->isLoggedIn() && $ownerId == $customerSession->getId()
            ? $customerSession->getCustomer()->getStoreId()
            : Mage::app()->getStore()->getId();
 
        $select = $this->_read->select()
            ->from($this->getMainTable())
            ->where('subscriber_email=:subscriber_email')
            ->where('store_id=:store_id'); // Add store ID for newsletters
 
        $result = $this->_read->fetchRow($select, array(
            'subscriber_email'  => $subscriberEmail,
            'store_id'          => $storeId
        ));
 
        if (!$result) {
            return array();
        }
 
        return $result;
    }
 
    /**
     * Load subscriber by customer
     *
     * @param Mage_Customer_Model_Customer $customer
     * @return array
     */
    public function loadByCustomer(Mage_Customer_Model_Customer $customer)
    {
        $select = $this->_read->select()
            ->from($this->getMainTable())
            ->where('customer_id=:customer_id')
            ->where('store_id=:store_id');
 
        $result = $this->_read->fetchRow($select, array(
            'customer_id'   => $customer->getId(),
            'store_id'      => $customer->getStoreId()
        ));
 
        if ($result) {
            return $result;
        }
 
        $select = $this->_read->select()
            ->from($this->getMainTable())
            ->where('subscriber_email=:subscriber_email')
            ->where('store_id=:store_id');
 
        $result = $this->_read->fetchRow($select, array(
            'subscriber_email'  => $customer->getEmail(),
            'store_id'          => $customer->getStoreId()
        ));
 
        if ($result) {
            return $result;
        }
 
        return array();
    }
}
