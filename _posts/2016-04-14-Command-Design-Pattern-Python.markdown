---
layout: post
title:  "Command Design Pattern using Python"
date:   2016-04-14 16:41:36 -0600
categories:
---
<style>
p {
  text-align: justify
}</style>

Command Design Pattern is one of the most helpful design patterns to learn. The best example for the organization of the command Design Pattern, would be that of a TV and remote control setup. Observing the structure of the Command Design Pattern, the most important structural blocks of the pattern are the Controller, Receiver, Invoker, and the Command Interface. As an example, to explain the command design pattern, I would use one of the code snippets from my recent Django Projects. I chose this as an example, as it is really practical and the pattern can be applied in similar scenarios like the one that we are going to discuss about.

`Example used :` A simple delete and undo operation of an entry in an application like - say an address book or better still a mail entry in your inbox.

<h4>Components of the command design pattern</h4>
The first component is a class called the `CommandInterface` that has a single method that is called `execute()` which is an abstract method.

{% highlight ruby %}

class CommandInterface():
	__metaclass__ = ABCMeta

	@abstractmethod
	def execute(self):pass

{% endhighlight %}

Next, we have <b>concrete command classes</b> as shown below. In our case, we want to delete an expense and also undo that action. As both the classes implement the <b>CommandInterface</b>, as a result, the abstract method `execute()` has to be implemented in both the classes. The `execute()` method will have the corresponding actions of either deleting an expense or undoing an expense. Let us look into the code snippet below and trace where the control flow goes.

{% highlight ruby %}

#concrete commands classes one for deleting the expenses and the other for undoing


class DeleteExpenseCommand(CommandInterface):
	def execute(self):
		receiver.deleteAll()


class UndoExpenseCommand(CommandInterface):
	def execute(self):
		receiver.undo_expenses()


{% endhighlight %}

The execute method accesses the `deleteAll()` method and the `undo_expenses()` method in the receiver class. This brings us to the next part of the command pattern - the <b>Receiver Class</b>. The class `ExpenseReceiver` actually consists of those operations that are responsible for data manipulation. In other words, this is the class which is responsible for changing the state of the data. In our case, the expense is deleted or the instance of the deleted expense is undone. Let us  swallow the logical block as a whole, just for the sake of an example. I understand that the logical coherence may be not quite apparent unless we have the entire application code! This can over complicate things, so let's use what we have here!

{% highlight ruby %}
class ExpenseReceiver():

	def deleteAll(self):
		#write logic here
		# instance = get_object_or_404(Expenses, deleted= 'true')
		# instance.delete()
	 	# return redirect("/expenses/detail")
	 	Expenses.objects.filter(deleted= "true").delete()

	def undo_expenses(self):
		#write logic here
		# instance = Expenses.objects.filter(deleted= "true").order_by("-date")
		# instance = get_object_or_404(Expenses, deleted= 'true')
		# setattr(instance, "deleted","")
		Expenses.objects.filter(deleted= "true").update(deleted = "")

{% endhighlight %}

Now, the next class is the Invoker class. Before diving into the working of the invoker class, one useful thing to realize about the application of design patterns, is that <b>it is alright if patterns are not present in their intrinsic form - design patterns are slightly modified to suit the UI and other control requirements!</b>. In our case, the invoker class is slightly modified. In my application, <b>invoker</b> is just a control from the view (a webpage). So, I have two methods called `expenses_undone()` and `expenses_deleteall()` which act as invokers. These are not inside an invoker class, but, you can add them inside an invoker class and then in turn call them inside your main user control method. I thought that this would create code redundancy so I have used my the structure as shown in this example.

The methods now call the corresponding methods in the `ExpenseReceiver` class to execute the corresponding logic. `Remember, the Receiver() class has to instantiated as receiver = ExpenseReceiver()` in order to access these methods. In Python this is done at the end of the program. Be sure to add this in.

{% highlight ruby %}

def expenses_undone(self):
	undo_expenses = UndoExpenseCommand()
	undo_expenses.execute()
	return redirect("/expenses/detail")

def expenses_deleteall(self):
	delete_expenses = DeleteExpenseCommand()
	delete_expenses.execute()
	return redirect("/expenses/detail")

{% endhighlight %}

These methods pass on the control to the `DeleteExpenseCommand` and the `UndoExpenseCommand` concrete classes, which later call the `execute()` method on the Receiver. As a result, the logic gets executed in a way similar to that of a TV remote controller. Now, that is quite awesome right? There might be slight moderations to the structure of this pattern, but this is the logical organization and flow of control of the Command pattern.

<br>
<b>Do you find any typos or disagree with something on this page? Let me know!</b>
