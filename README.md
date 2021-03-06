import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.Collections;
import javax.swing.BorderFactory;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;
//Md Rashedul
public class AttendanceLogTest extends JFrame implements ActionListener {
// declaring components
	private JTextField status;
	private ArrayList<PersonAttend> list;
	private JTextArea output;
	private JButton addBtn, searchBtn, sortBtn, deleteBtn;

	public AttendanceLogTest() {
		setDefaultCloseOperation(EXIT_ON_CLOSE);
// initializing the list
		list = new ArrayList<PersonAttend>();
		status = new JTextField("Output of operations will appear here.");
		status.setEditable(false);
// creating a Panel to hold the textarea to display the list
		JPanel outputPanel = new JPanel();
// setting a title border
		outputPanel.setBorder(BorderFactory.createTitledBorder("Person List"));
// initializing text area which will display the person list
		output = new JTextArea(10, 40);
		output.setEditable(false);
		outputPanel.add(output);
// creating a panel for buttons and initializing buttons
		JPanel buttonsPanel = new JPanel();
		addBtn = new JButton("Add");
		searchBtn = new JButton("Search");
		sortBtn = new JButton("Sort");
		deleteBtn = new JButton("Delete");
		buttonsPanel.add(addBtn);
		buttonsPanel.add(searchBtn);
		buttonsPanel.add(sortBtn);
		buttonsPanel.add(deleteBtn);
// using border layout to arrange elements
		setLayout(new BorderLayout());
// adding each components in proper positions
		add(status, BorderLayout.PAGE_START);
		add(outputPanel, BorderLayout.CENTER);
		add(buttonsPanel, BorderLayout.PAGE_END);
		pack(); // assigning a compact size
// adding action listeners to the buttons
		addBtn.addActionListener(this);
		searchBtn.addActionListener(this);
		sortBtn.addActionListener(this);
		deleteBtn.addActionListener(this);
	}

// method to ask user for a name using a dialog box, returns the name
	public String getName() {
		String name = JOptionPane.showInputDialog("Enter full name: ");
		return name;
	}

// method to add a person to list, returns the result of operation
	public String addPerson(String name) {
		if (name == null) {
			return "Invalid name";
		}
		if (searchPerson(name) != null) {
			return "Person with same name exists";
		}
		String id = JOptionPane.showInputDialog("Enter Id or telephone number: ");
		if (id == null) {
			return "Invalid Id number";
		}
		String email = JOptionPane.showInputDialog("Enter email: ");
		if (email == null) {
			return "Invalid email";
		}
		// creating a Person and adding to the list
		PersonAttend p = new PersonAttend(name, id, email);
		list.add(p);
		return "Added successfully";
	}

// method to search for a person by name
	public PersonAttend searchPerson(String name) {
		if (name == null) {
			return null;
		}
		for (PersonAttend p : list) {
			if (p.getName().equalsIgnoreCase(name)) {
				return p;
			}
		}
		return null;
	}

// method to sort list by name
	public void sort() {

		/**
		 * The Person class is Comparable, just call built in sort method
		 */

		Collections.sort(list);

	}

	public boolean removePerson(String name) {

		if (name == null) {

			return false;
		}
		for (int i = 0; i < list.size(); i++) {
			if (list.get(i).getName().equalsIgnoreCase(name)) {
				list.remove(i);
				return true;
			}
		}
		return false;
	}

	public static void main(String[] args) {
// creating PersonGUI object and making it visible
		AttendanceLogTest gui = new AttendanceLogTest();
		gui.setVisible(true);
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		Object ob = e.getSource();
		if (ob.equals(addBtn)) {
// getting name, calling addPerson
			String name = getName();
			String result = addPerson(name);
// setting status textfield with result of operation
			status.setText(result);
		} else if (ob.equals(searchBtn)) {
			String name = getName();
			PersonAttend result = searchPerson(name);
			if (result == null) {
				status.setText("Specified person is not found!");
			} else {
				status.setText("Search result:\t" + result);
			}
		} else if (ob.equals(sortBtn)) {
			sort();
			status.setText("Sorted by name!");
		} else if (ob.equals(deleteBtn)) {
			String name = getName();
			if (removePerson(name)) {
				status.setText("Removal successful!");
			} else {
				status.setText("Specified person is not found!");
			}
		}
// displaying the person list on text area
		String listText = "";
		for (PersonAttend p : list) {
			listText += p.toString() + "\n";
		}
		output.setText(listText);
	}

}
