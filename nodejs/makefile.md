# Makefile #


```
#
# Makefile
#
# Start with @ to hide source
#
# If you get "*** missing separator.  Stop.", it means you have spaces where 
# there should be a tab
#
# Tips:
#   - For $ signs, you'll need to escape with $ (ex. $$)
#   - $! gets the PID of the last process
# 
# @author Eugene Song <esong@tesla.com>
#
# 

NODE_ENV?=production
ENTRY_POINT=`echo "process.stdout.write(require('./package.json').main)" | node`
ENV_DECRYPTED_PATH="config/env/${NODE_ENV}.env"
ENV_ENCRYPTED_PATH="config/env/${NODE_ENV}.env.gpg"
RECIPIENTS_PATH="config/recipients/${NODE_ENV}.txt"
LOG_LOCATION="var/all.log"
PM2_NAME?=`echo "process.stdout.write(require('./package.json').name)" | node`
PM2_INSTANCES?=0

.PHONY: decrypt encrypt run start develop test-coverage

# Alternative
#gpg --output - --decrypt ${ENV_ENCRYPTED_PATH} | tee ${ENV_DECRYPTED_PATH}
decrypt:
	@gpg --output ${ENV_DECRYPTED_PATH} --decrypt ${ENV_ENCRYPTED_PATH}

encrypt:
	@test -s ${RECIPIENTS_PATH} || echo "Recipients not defined in file: ${RECIPIENTS_PATH}"
	@test -s ${RECIPIENTS_PATH} || exit 1
	gpg --yes --output ${ENV_ENCRYPTED_PATH} --encrypt `cat ${RECIPIENTS_PATH} | xargs -I '{}' echo "--recipient" {}` ${ENV_DECRYPTED_PATH}
	
	
run:
	#	gpg --decrypt -q 2>>${LOG_LOCATION} | xargs -I {} echo "export "\{\} 
	gpg --decrypt -q ${ENV_ENCRYPTED_PATH}


start:
	@test -s ${ENV_DECRYPTED_PATH} || exit 1
	eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` node ${ENTRY_POINT} 

develop:
	test -s ${ENV_DECRYPTED_PATH} || exit 1
	eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` nodemon ${ENTRY_POINT}

pm2:
	test -s ${ENV_DECRYPTED_PATH} || exit 1
	eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` pm2 start ${ENTRY_POINT} -i ${PM2_INSTANCES} --name ${PM2_NAME}
	

test-coverage:
	eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` ./node_modules/.bin/tap lib/**/*.test.js --coverage
  
```



```
NODE_ENV?=production
APP_ENV?=${NODE_ENV}
ENTRY_POINT=`echo "process.stdout.write(require('./package.json').main)" | node`
ENV_DECRYPTED_PATH="config/env/${APP_ENV}.env"
ENV_ENCRYPTED_PATH="config/env/${APP_ENV}.env.gpg"
RECIPIENTS_PATH="config/recipients/${APP_ENV}.txt"
LOG_LOCATION="var/all.log"
PM2_NAME?=`echo "process.stdout.write(require('./package.json').name)" | node`
PM2_INSTANCES?=0

.PHONY: decrypt encrypt run start develop test-coverage

# Alternative
#gpg --output - --decrypt ${ENV_ENCRYPTED_PATH} | tee ${ENV_DECRYPTED_PATH}
decrypt:
	@gpg --output ${ENV_DECRYPTED_PATH} --decrypt ${ENV_ENCRYPTED_PATH}

encrypt:
	@test -s ${RECIPIENTS_PATH} || echo "Recipients not defined in file: ${RECIPIENTS_PATH}"
	@test -s ${RECIPIENTS_PATH} || exit 1
	gpg --yes --output ${ENV_ENCRYPTED_PATH} --encrypt `cat ${RECIPIENTS_PATH} | xargs -I '{}' echo "--recipient" {}` ${ENV_DECRYPTED_PATH}
	
run:
	#	gpg --decrypt -q 2>>${LOG_LOCATION} | xargs -I {} echo "export "\{\} 
	gpg --decrypt -q ${ENV_ENCRYPTED_PATH}

start:
	#@test -s ${ENV_DECRYPTED_PATH} || exit 1
	#eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` node ${ENTRY_POINT}
	./bin/env ${APP_ENV} node ${ENTRY_POINT}

develop:
	#test -s ${ENV_DECRYPTED_PATH} || exit 1
	#eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` nodemon ${ENTRY_POINT}
	./bin/env ${APP_ENV} nodemon ${ENTRY_POINT}

pm2:
	#test -s ${ENV_DECRYPTED_PATH} || exit 1
	#eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` pm2 start ${ENTRY_POINT} --name ${PM2_NAME} -i ${PM2_INSTANCES}
	./bin/env ${APP_ENV} pm2 start ${ENTRY_POINT} --name ${PM2_NAME} -i ${PM2_INSTANCES}

test-coverage:
	#eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` ./node_modules/.bin/tap lib/**/*.test.js --coverage
	./bin/env ${APP_ENV} ./node_modules/.bin/tap lib/**/*.test.js --coverage

```