# Functional-Coverage
About FC
What is FC?

    FC is used to keep track of the project status. It is the measure for deciding the robustness of test cases.
    Without FC it is difficult to keep track of the project status.
    It doesn’t tell whether the feature is working or not, it only tells that feature has been checked as part of the test cases.

How FC gets transactions?

    Monitor monitors the interface for valid txs and collects them.
    It puts transactions into mailbox connected with FC.
    FC block gets transaction from mailbox.
    FC block samples cover group for every transaction coming from monitor.

Coverage Hierarchy

    FC is coded into a class, each coverage class has

    One or more Cove groups.
    Each cover group consists of one or more cover points.
    Each cover point can be implemented on multiple bins.
    Each bin is either single value or multiple set of values

    When one of the values in bin is hit, then that bin is set to be covered.
    %FC = (Bins covered/Total bins)*100%

%FC cover group = (sum of all cover points/total number of cover points)
Overall FC = (sum of all cover group in class/total number of cover group)

Types of Bins

    Bins: Using “bins” keyword we can declare explicit bins to a variable. Bins will be included for coverage calculation.

◦Example: WR_RD_CP:coverpoint tx.wr_rd{
Bins WR_BIN = {1};
Bins RD_BIN = {0};
}

    Illegal Bins:
    ◦ Not used in coverage calculation.
    ◦If anytime this bin happens, tool will report run time error.
    ◦Example:
    illegal_bins ILL_ADDR = {[50:$]};
    illegal addr bin is said to be covered when anyone value ranging from 50 to 255 occurs.

    Ignore Bins:
    ◦Not used in coverage calculation.
    ◦If this bin is hit tool will not report any error.
    ◦We use this bins when we know a particular scenario that should be generated.
    ◦ Example: WR_RD_CP:coverpoint tx.wr_rd{
    Bins WR_BIN = {1};
    Ignore bin IG_WR_BIN={0};
    Bins RD_BIN = {0};
    Ignore bin IG_RD_BIN={1};
    }

    Wildcard Bins:

◦Bins are declared using wildcard keyword
◦We use this bin when we want to write X or Z or ? During bins declaration.
◦Example:
Coverpoint a{
Wildcard bins b1=(2’b0X => 2’b1X)
}
B1 is set to be hit whenever anyone of below transition happens
01 -> 10, 00 -> 11, 01 -> 11, 00 -> 10

    Cross Coverage:

◦It will have cross product between two or more cover points within the same cover group.
◦Example:
WR_RD_CP_X_ADDR_CP:cross WR_RD_CP,ADDR_CP;

WR_RD_CP and ADDR_CP are two cover points. Cross product of these cover points gives the cross coverage.

    Cross Coverage With Intersect:

◦Analysing more number of bins is a difficult task, for easier analysis we want to minimize them into smaller number of bins.
◦Intersect will help us to group a specific set of values into a single bin.
◦ Example:
A_CP:coverpoint a{
bins A0 = {0};
bins A1 = {1};
}
B_CP:coverpoint b{
bins B0 = {0};
bins B1 = {1};
}
A_X_B_1_intersect:cross A_CP,B_CP{
bins BIN1 = binsof(A_CP) intersect{1};
}
◦Total 4 bin combinations will be formed i.e,
A0B0, A0B1, A1B0, A1B1.
While using intersect concept we can merge the A_CP which holds the bins 1.
So A1B0, A1B1 will be merged as 1 bin.
There will be total of 3 bins. They are
A0B0, A0B1, Intersect bin{A1B0, A1B1}.
