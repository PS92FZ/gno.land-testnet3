# gnoland
git clone git@github.com:gnolang/gno.git
cd ./gno
make 
./build/gnokey add test1 --recover

./build/gnoland

go run ./gnoland/website

./build/gnokey maketx addpkg test1 --pkgpath "gno.land/p/avl" --pkgdir "examples/gno.land/p/avl" --deposit 100ugnot --gas-fee 1ugnot --gas-wanted 2000000 > addpkg.avl.unsigned.txt
./build/gnokey query "auth/accounts/g1jg8mtutu9khhfwc4nxmuhcpftf0pajdhfvsqf5"
./build/gnokey sign test1 --txpath addpkg.avl.unsigned.txt --chainid "test2" --number 0 --sequence 0 > addpkg.avl.signed.txt
./build/gnokey broadcast addpkg.avl.signed.txt --remote test2.gno.land:26657

./build/gnokey maketx addpkg test1 --pkgpath "gno.land/r/boards" --pkgdir "examples/gno.land/r/boards" --deposit 100ugnot --gas-fee 1ugnot --gas-wanted 300000000 > addpkg.boards.unsigned.txt
./build/gnokey sign test1 --txpath addpkg.boards.unsigned.txt --chainid "test2" --number 0 --sequence 1 > addpkg.boards.signed.txt
./build/gnokey broadcast addpkg.boards.signed.txt --remote test2.gno.land:26657

./build/gnokey maketx call test1 --pkgpath "gno.land/r/boards" --func CreateBoard --args "testboard" --gas-fee 1ugnot --gas-wanted 2000000 > createboard.unsigned.txt
./build/gnokey sign test1 --txpath createboard.unsigned.txt --chainid "test2" --number 0 --sequence 2 > createboard.signed.txt
./build/gnokey broadcast createboard.signed.txt --remote test2.gno.land:26657

./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"testboard\")"

./build/gnokey maketx call test1 --pkgpath "gno.land/r/boards" --func CreatePost --args 1 --args "Hello World" --args#file "./examples/gno.land/r/boards/README.md" --gas-fee 1ugnot --gas-wanted 2000000 > createpost.unsigned.txt
./build/gnokey sign test1 --txpath createpost.unsigned.txt --chainid "test2" --number 0 --sequence 3 > createpost.signed.txt
./build/gnokey broadcast createpost.signed.txt --remote test2.gno.land:26657

./build/gnokey maketx call test1 --pkgpath "gno.land/r/boards" --func CreateReply --args 1 --args 1 --args "A comment" --gas-fee 1ugnot --gas-wanted 2000000 > createcomment.unsigned.txt
./build/gnokey sign test1 --txpath createcomment.unsigned.txt --chainid "test2" --number 0 --sequence 4 > createcomment.signed.txt
./build/gnokey broadcast createcomment.signed.txt --remote test2.gno.land:26657
./build/gnokey query "vm/qrender" --data "gno.land/r/boards
testboard/1"

./build/gnokey query "vm/qrender" --data "gno.land/r/boards
testboard"
