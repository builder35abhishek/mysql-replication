# mysql-replication






server_id           = 1
log_bin             = /var/log/mysql/mysql-bin.log
log_bin_index       = /var/log/mysql/mysql-bin.log.index
relay_log           = /var/log/mysql/mysql-relay-bin
relay_log_index     = /var/log/mysql/mysql-relay-bin.index
log_slave_updates   = 1


server_id           = 2
log_bin             = /var/log/mysql/mysql-bin.log
log_bin_index       = /var/log/mysql/mysql-bin.log.index
relay_log           = /var/log/mysql/mysql-relay-bin
relay_log_index     = /var/log/mysql/mysql-relay-bin.index
log_slave_updates   = 1

server_id           = 3
log_bin             = /var/log/mysql/mysql-bin.log
log_bin_index       = /var/log/mysql/mysql-bin.log.index
relay_log           = /var/log/mysql/mysql-relay-bin
relay_log_index     = /var/log/mysql/mysql-relay-bin.index
log_slave_updates   = 1

STOP SLAVE;
SET GLOBAL master_info_repository = 'TABLE';
SET GLOBAL relay_log_info_repository = 'TABLE';


GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%' IDENTIFIED BY 'BN#2018PL';

master 2
STOP SLAVE;
CHANGE MASTER TO MASTER_HOST='13.232.172.26', MASTER_USER='repel', MASTER_PORT=3306, MASTER_PASSWORD='BN#2018PL', MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=154 FOR CHANNEL 'master-2';
START SLAVE;

master 1
STOP SLAVE;
CHANGE MASTER TO MASTER_HOST='13.233.143.219', MASTER_USER='repel', MASTER_PORT=3306, MASTER_PASSWORD='BN#2018PL', MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=154 FOR CHANNEL 'master-1';
START SLAVE;

m1 13.232.172.26
m2 13.233.143.219



START SLAVE thread_types FOR CHANNEL channel;

slave > START SLAVE FOR CHANNEL 'master-1';
slave > START SLAVE FOR CHANNEL 'master-2';

slave1
CHANGE MASTER TO MASTER_HOST='13.232.172.26', MASTER_USER='repel', MASTER_PORT=3306, MASTER_PASSWORD='BN#2018PL', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=315 FOR CHANNEL 'master-2';
CHANGE MASTER TO MASTER_HOST='13.233.143.219', MASTER_USER='repel', MASTER_PORT=3306, MASTER_PASSWORD='BN#2018PL', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=315 FOR CHANNEL 'master-1';


