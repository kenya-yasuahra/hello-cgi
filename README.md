#!/usr/local/bin/perl
# hello2.cgi -- 

print "Content-type: text/html\n\n";

read(STDIN, $str, $ENV{"CONTENT_LENGTH"});

@parts = split('&', $str);
foreach(@parts) {
       ($variable, $value) = split("=");
       $value =~ tr/+/ /;
       $value =~ s/%([0-9a-fA-F][0-9a-fA-F])/pack("C", hex($1))/eg;
       $value =~ s/\r\n/\n/g;
       $value =~ s/\r/\n/g;
       $cgi{$variable} = $value;
}
$formfile = "./chatform.htm";

$name = $cgi{'name'};
$memo = $cgi{'memo'};

print <<EOF;
<META HTTP-EQUIV="Refresh"
CONTENT = "O;URL=http://local.kisc.meiji.ac.jp/~eg40346/chatform.htm"> #�e����URI�ɕύX���邱��
<body bgcolor=silver text=navy>
wait for a second...<P>
EOF

open(FILE, "$formfile");
@oldform = <FILE>;
close(FILE);

$li = undef;
foreach(@oldform) {
        if(/^<DT>/) {
                @body = (@body, $_);
                $li = 1;
        } elsif ($li) {
                @foot = (@foot, $_);
        } elsif {
                @head = (@head, $_);
        }
}

open(FILE, ">$formfile");
select FILE;
print @head;
print @body;
print <<EOF;
<DT><B>$name</B><DD>$memo
EOF
print @foot;
close(FILE);
