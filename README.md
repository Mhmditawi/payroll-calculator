# payroll-calculator
payroll-calculator 
import 'package:flutter/material.dart';

void main() {
  runApp(PayrollCalculatorApp());
}

class PayrollCalculatorApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: PayrollCalculator(),
    );
  }
}

class PayrollCalculator extends StatefulWidget {
  @override
  _PayrollCalculatorState createState() => _PayrollCalculatorState();
}

class _PayrollCalculatorState extends State<PayrollCalculator> {
  // Controllers for input fields
  final TextEditingController grossSalaryController = TextEditingController();
  final TextEditingController taxRateController = TextEditingController();
  final TextEditingController deductionsController = TextEditingController();

  // Variables for results
  double grossSalary = 0.0;
  double taxAmount = 0.0;
  double otherDeductions = 0.0;
  double netPay = 0.0;

  // Calculation function
  void calculateNetPay() {
    setState(() {
      grossSalary = double.tryParse(grossSalaryController.text) ?? 0.0;
      double taxRate = double.tryParse(taxRateController.text) ?? 0.0;
      otherDeductions = double.tryParse(deductionsController.text) ?? 0.0;

      // Validate inputs
      if (grossSalary < 0 || taxRate < 0 || otherDeductions < 0) {
        showDialog(
          context: context,
          builder: (_) => AlertDialog(
            title: Text('Invalid Input'),
            content: Text('Please enter positive values for all fields.'),
            actions: [
              TextButton(
                onPressed: () => Navigator.pop(context),
                child: Text('OK'),
              )
            ],
          ),
        );
        return;
      }

      taxAmount = (grossSalary * taxRate) / 100;
      netPay = grossSalary - taxAmount - otherDeductions;
    });
  }

  // Reset function
  void resetFields() {
    setState(() {
      grossSalaryController.clear();
      taxRateController.clear();
      deductionsController.clear();
      grossSalary = 0.0;
      taxAmount = 0.0;
      otherDeductions = 0.0;
      netPay = 0.0;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Payroll and Tax Calculator'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              TextField(
                controller: grossSalaryController,
                decoration: InputDecoration(
                  labelText: 'Gross Salary',
                  helperText: 'Enter your gross salary before taxes.',
                  errorText: grossSalary < 0 ? 'Invalid value' : null,
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                controller: taxRateController,
                decoration: InputDecoration(
                  labelText: 'Tax Rate (%)',
                  helperText: 'Enter the tax rate as a percentage (e.g., 20 for 20%).',
                  errorText: grossSalary < 0 ? 'Invalid value' : null,
                ),
                keyboardType: TextInputType.number,
              ),
              TextField(
                controller: deductionsController,
                decoration: InputDecoration(
                  labelText: 'Other Deductions',
                  helperText: 'Enter other deductions such as insurance, etc.',
                ),
                keyboardType: TextInputType.number,
              ),
              SizedBox(height: 20),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  ElevatedButton(
                    onPressed: calculateNetPay,
                    child: Text('Calculate'),
                  ),
                  ElevatedButton(
                    onPressed: resetFields,
                    child: Text('Reset'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.red,
                    ),
                  ),
                ],
              ),
              SizedBox(height: 20),
              Text(
                'Results:',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              Text('Gross Salary: \$${grossSalary.toStringAsFixed(2)}'),
              Text('Tax Amount: \$${taxAmount.toStringAsFixed(2)}'),
              Text('Other Deductions: \$${otherDeductions.toStringAsFixed(2)}'),
              Text('Net Pay: \$${netPay.toStringAsFixed(2)}'),
            ],
          ),
        ),
      ),
    );
  }
}
