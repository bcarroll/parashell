#parashell  -- Brett Carroll @ 2010
#Free to use for whatever you like.  If you use parashell, please add feedback to the parashell project forum on sourceforge.net
#Read and Write data to/from the parallel port as recieved from the cmdline.
#Usage: parashell.exe [OPTIONS]
#Simple parallel port input/output control program for Windows

use strict;
use warnings;
use Device::ParallelPort; #If missing: ppm install Device-ParallelPort
use Getopt::Long;

my $parport = Device::ParallelPort->new('win32'); #load the win32 parallel port driver

my ($help,$verbose);

#Initialize variables to avoid errors/warnings if using warnings.pm
my $data = undef; #set default data to null
my $mode = 'r'; #set default mode to read
my $port = 'd'; #set default port to DATA

#print usage statement if no command line parameters are passed or there is an unknown parameter or help option is passed
GetOptions('mode|m=s' => \$mode, 'port|p=s' => \$port, 'data|d:i' => \$data, 'help|h|?' => \$help, 'verbose|v' => \$verbose);

if ($help){
	&usage;
};

if ($mode =~ /r/i){
	print "MODE = READ\n" if $verbose;
	print pin($port); #read from the specified port
} elsif ($mode =~ /w/i){
	print "MODE = WRITE\n" if $verbose;
	pout ($port, $data); #write to the specified port
}

sub pin {
	my $port = shift;
	my $val;
	if ($port =~ /c/i){
		$val = $parport->get_byte(1); #read from the CONTROL port
	} elsif ($port =~ /d/i){
		$val = $parport->get_byte(0); #read from the DATA port
	} elsif ($port =~ /s/i){
		$val = $parport->get_byte(2); #read from the STATUS port
	} else {
		usage();
	}
	print "PORT = " . port($port) . "\n" if $port && $verbose;
	print "DATA = " . ord($val) . "\n" if $verbose;
	return ord($val);
}

sub pout {
	my ($port, $data) = @_;
	&usage unless $port && $data;
	print "PORT = " . port($port) . "\n" if $port && $verbose;
	print "DATA = $data\n" if $verbose;
	my $val;
	for my $i (0 .. 255){ #This loop verifies the data entered is between 0 and 255
		if ($i == $data){
			if ($port =~ /c/i){
				$parport->set_byte(1,chr($data)); #write to the CONTROL port
			} elsif ($port =~ /d/i){
				$parport->set_byte(0,chr($data)); #write to the DATA port
			} elsif ($port =~ /s/i){
				$parport->set_byte(2,chr($data)); #write to the STATUS port
			}
			return 0;
		}
	}
}

sub usage {
	#Usage statement
	print "Usage: parashell.exe [OPTIONS]\n\n";
	print "Options:\n";
	print "-mode|m\t\tSet parashell mode (read or write)\n";
	print "    r\t\t    Set read mode\n";
	print "    w\t\t    Set write mode\n\n";
	print "-port|p\t\tSet port to use\n";
	print "    c\t\t    Use CONTROL port\n";
	print "    d\t\t    Use DATA port\n";
	print "    s\t\t    Use STATUS port\n\n";
	print "-data|d\t\tSpecifies data to be sent to the specified port (-mode must be set to w)\n";
	print "\t\tdata can be decimal (0-255)\n\n";
	print "Example:\n";
	print "\tc:\\>parashell.exe -mode w -port d -data 255\n";
	print "\tThis example sets all 8 DATA bits high\n";
	exit;
}

sub port{
	my $port = shift;
	my %ports = (
		'c' => 'CONTROL',
		'C' => 'CONTROL',
		'd' => 'DATA',
		'D' => 'DATA',
		's' => 'STATUS',
		'S' => 'STATUS'
	);
return $ports{$port};
}