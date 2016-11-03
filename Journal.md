# Journal

* When writing / storing:
	* M_cmd:	b_out = 1(write) + address
	* M_data: a_out = data_to_write
* When reading / loading:
	* M_cmd: cmdmux = 0
	* M_data: a_out = address

* Bugs:
	* 	How do we make sure b_out has a "1" as MSB and address after.
	*  fucking swag