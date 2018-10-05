# Получение списка почтовых служб работающих с почтоматами

Clone [folder "Getting the list of postal services working with automated parcel terminals"](https://admin.corezoid.com/folder/conv/6081) to get the process and dashboard.

![](../img/copy_folder.png)

Go to the process.

For testing the process, go to the mode `dashboard` and click `Add task` to add the request.

![](../img/mandrill_dashboard.png)

Then press the button `Send task` - to send the request.

**In case of success** the following parameter is added to the request:

* `Pochtomats_List`- list of postal services working with automated parcel terminals

**In case of error** the request goes to the escalation node with the parameter below:
* `Error` - Error description