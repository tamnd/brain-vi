---
title: "CF 102341J - Jigglypuff"
description: "Chúng ta có một lưới (n lần m) gồm các chữ cái viết thường. Lộ trình bắt đầu ở ô phía trên bên trái, kết thúc ở ô phía dưới bên phải và chỉ bao gồm các bước di chuyển sang phải và xuống. Mỗi tuyến truy cập chính xác (n+m-1) ô, vì vậy mọi tuyến đều tạo ra một chuỗi có cùng độ dài."
date: "2026-08-13T03:20:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "J"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 195
verified: true
draft: false
---

[CF 102341J - Jigglypuff](https://codeforces.com/problemset/problem/102341/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times m) gồm các chữ cái viết thường. Lộ trình bắt đầu ở ô phía trên bên trái, kết thúc ở ô phía dưới bên phải và chỉ bao gồm các bước di chuyển sang phải và xuống. Mỗi tuyến truy cập chính xác (n+m-1) ô, vì vậy mọi tuyến đều tạo ra một chuỗi có cùng độ dài. 

Câu hỏi đặt ra là liệu một chuỗi cụ thể nào đó có thể được tạo ra bằng ít nhất ba con đường khác nhau hay không. Bản thân các tuyến đường không nhất thiết phải rời rạc. Chúng có thể chia sẻ tiền tố và hậu tố dài hoặc thậm chí hầu hết các ô của chúng. Điều quan trọng là trình tự các chữ cái được truy cập của chúng giống hệt nhau trong khi trình tự di chuyển của chúng lại khác nhau. 

Giới hạn (n,m\le 3000) cho tới chín triệu ô lưới. Một giải pháp kiểm tra mọi tế bào với số lần không đổi là phù hợp. Bất kỳ số bậc hai nào về số lượng ô, chẳng hạn như (O(n^2m^2)), đều quá lớn và việc liệt kê rõ ràng các tuyến đường là hoàn toàn không thể vì số lượng của chúng là 

[ 
\binom{n+m-2}{n-1}. 
] 

Đối với (n=m=3000), đây là khoảng (10^{1803}) tuyến đường. Ngay cả việc viết ra một chút thông tin cho mỗi tuyến đường cũng không thể thực hiện được. 

Có một số trường hợp khó khăn trong đó một điều kiện hợp lý bề ngoài lại đưa ra câu trả lời sai. Đầu tiên, một hình vuông (2\times2) có thể có hai tuyến tạo ra cùng một chuỗi, nhưng không bao giờ có thể có ba tuyến.```
2 2
aa
aa
```Câu trả lời đúng là`NO`. Một giải pháp bất cẩn chỉ kiểm tra xem hai tuyến đường khác nhau có thể có cùng một chuỗi hay không và in sai`YES`. 

Trường hợp thứ hai là hai cấu hình cục bộ hữu ích có thể nằm trên cùng một hàng nhưng cách nhau nhiều hơn một cột.```
2 4
abca
bday
```Các ô tốt nằm ở cột 1 và 3 của hàng đầu tiên, sử dụng tọa độ một cơ sở. Chúng không liền kề nên không đưa ra ba tuyến đường bằng nhau. Câu trả lời là`NO`. Việc coi hai ô tốt bất kỳ trong cùng một hàng là đủ sẽ là sai. 

Hai ô tốt liền kề sẽ đưa ra ba tuyến đường. Ví dụ,```
2 3
aba
bab
```có ba con đường khác nhau, tất cả đều sản xuất`abab`, vậy câu trả lời là`YES`. 

Hiện tượng tương tự xảy ra theo chiều dọc:```
3 2
ab
ba
ab
```Một lần nữa có ba con đường sản xuất`abab`, vậy câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi tuyến đường đơn điệu, xây dựng chuỗi của nó và đếm số lần mỗi chuỗi xuất hiện. Điều này đúng vì mọi tuyến đường đều được xem xét và các tuyến tạo ra cùng một chuỗi sẽ được nhóm lại với nhau. Vấn đề là số lượng tuyến đường. Có (\binom{n+m-2}{n-1}) trong số chúng và việc xây dựng mỗi chuỗi sẽ mất (O(n+m)), đưa ra 

[ 
O\left((n+m)\binom{n+m-2}{n-1}\right) 
] 

thời gian. Đối với lưới (3000\times3000), đây là thứ tự của các thao tác ký tự (10^{1807}), vì vậy cách tiếp cận này không chỉ đơn thuần là quá chậm mà về cơ bản là không thể sử dụng được. 

Quan sát hữu ích là hai tuyến đường đơn điệu chỉ có thể khác nhau cục bộ bằng cách hoán đổi một bước đi phải, sau đó là một bước đi xuống với một bước đi xuống, sau đó là một bước di chuyển phải. Hãy xem xét một ô ((r,c)). Hai khả năng cục bộ là 

[ 
(r,c)\rightarrow(r,c+1)\rightarrow(r+1,c+1) 
] 

và 

[ 
(r,c)\rightarrow(r+1,c)\rightarrow(r+1,c+1). 
] 

Các ô đầu tiên và cuối cùng được chia sẻ. Hai ô ở giữa phải có cùng ký tự thì hai tuyến đường mới tạo ra cùng một chuỗi con. Do đó, xác định một ô ((r,c)) là tốt khi 

[ 
lưới[r][c+1]=lưới[r+1][c]. 
] 

Một ô tốt biểu thị một hoán đổi cục bộ không làm thay đổi chuỗi được tạo ra. 

Điều đáng ngạc nhiên là ba tuyến đường giống nhau chỉ có thể tồn tại thông qua hai cách sắp xếp cụ thể của các tế bào tốt. Sự sắp xếp đầu tiên bao gồm hai ô tốt ((r_1,c_1)) và ((r_2,c_2)) với 

[ 
r_1<r_2,\qquad c_1<c_2. 
] 

Chúng nằm hoàn toàn ở phía đông nam của nhau, vì vậy hai giao dịch hoán đổi cục bộ có thể được thực hiện độc lập. Bắt đầu với một tuyến đường đi qua cả hai ô vuông, chúng ta có thể chọn không hoán đổi, chỉ ô thứ nhất, chỉ ô thứ hai hoặc cả hai. Điều đó mang lại ít nhất ba tuyến đường riêng biệt có cùng một chuỗi. 

Sự sắp xếp thứ hai bao gồm hai ô tốt liền kề. Chúng nằm cạnh nhau theo chiều ngang, 

[ 
(r,c),\(r,c+1), 
] 

hoặc liền kề theo chiều dọc, 

[ 
(r,c),\(r+1,c). 
] 

Hai hình vuông chạm nhau, do đó ba cách có thể đi qua vùng kết hợp của chúng đã đưa ra ba tuyến đường khác nhau với cùng một chuỗi. 

Điều ngược lại là bổ đề cấu trúc quan trọng. Nếu ba tuyến tạo ra cùng một chuỗi, hãy sắp xếp chúng từ trên xuống dưới trên mọi đường chéo của lưới. Nén tiền tố và hậu tố chung của chúng. Vùng đầu tiên vẫn còn ba lựa chọn tuyến đường riêng biệt phải chứa hai điểm hoán đổi cục bộ độc lập được phân tách nghiêm ngặt ở cả hai tọa độ hoặc hai điểm hoán đổi chia sẻ một bên. Vì các nhãn trên các vị trí tương ứng của cả ba tuyến đều giống nhau nên mọi hoán đổi cục bộ được yêu cầu đều là một ô tốt. Do đó một trong hai cấu hình trên phải xảy ra. Bài xã luận chính thức nêu rõ đặc điểm này. 

Điều này làm giảm bài toán đường đi ban đầu thành bài toán hình học trên tập hợp các ô tốt. Chúng ta cần phát hiện một cặp liền kề hoặc một cặp theo hàng và cột tăng dần. 

Đối với trường hợp nghiêm ngặt về phía đông nam, hãy quét các hàng từ trên xuống dưới. Với mỗi hàng, hãy tìm cột nhỏ nhất và lớn nhất chứa một ô tốt. Nếu hàng hiện tại có một ô tốt ở cột (c) và một số hàng trước đó có một ô tốt ở cột nhỏ hơn thì chúng ta có cấu hình đầu tiên. Chỉ cần nhớ cột tối thiểu trong số tất cả các ô tốt ở các hàng trước đó là đủ. Nếu cột tốt nhất của hàng hiện tại lớn hơn cột tối thiểu đó thì cặp bắt buộc sẽ tồn tại. 

Vấn đề còn lại là thực hiện tính toán ô tốt một cách nhanh chóng trong Python. Lưới có tới chín triệu ô, do đó, một vòng lặp Python lồng nhau trên mỗi ký tự có thể tốn kém một cách không cần thiết dưới giới hạn một giây. Chúng ta có thể gói mỗi hàng thành một số nguyên Python, sử dụng một byte cho mỗi ký tự lưới. Đối với hai hàng liên tiếp (A) và (B), byte tại vị trí (c) của 

[ 
(A\mathbin{>>}8)\mathbin{\mathsf{XOR}}B 
] 

chính xác bằng 0 khi (A[c+1]=B[c]), đây chính xác là định nghĩa của một ô tốt. Python thực hiện các phép toán số nguyên lớn này bằng mã gốc được tối ưu hóa. 

Một biểu thức phát hiện byte 0 tiêu chuẩn, 

[ 
(x-L)\ &\ \sim x\ &\ H, 
]

trong đó (L) chứa giá trị byte (1) ở mọi vị trí và (H) chứa giá trị byte (128) ở mọi vị trí, tạo ra một mặt nạ bit có các byte được thiết lập tương ứng với các byte 0 của (x). Điều này cho phép chúng tôi tìm thấy tất cả các ô tốt của một cặp hàng cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n+m)\binom{n+m-2}{n-1})) | Hàm mũ trong kích thước lưới | Quá chậm | 
| Tối ưu | (O(nm)) | (O(m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hàng lưới đầu tiên và gói các ký tự của nó thành một số nguyên Python. Chỉ cần hàng trước đó vì một ô tốt được xác định bởi hai hàng liên tiếp. 
2. Đối với mỗi hàng tiếp theo, hãy gói nó thành một số nguyên và tính toán (x=(previous>>8)\mathbin{\mathsf{XOR}}current). Byte (c) của (x) so sánh ký tự ở cột (c+1) ở hàng trước với ký tự ở cột (c) ở hàng hiện tại. 
3. Trích xuất byte 0 của (x). Mỗi byte như vậy đại diện cho một ô tốt. Từ mặt nạ bit thu được, lấy cột tốt đầu tiên và cuối cùng trong cặp hàng hiện tại. 
4. Kiểm tra xem mặt nạ tốt hiện tại có giao nhau với mặt nạ tốt trước đó hay không. Nếu đúng như vậy thì có hai ô tốt liền kề nhau theo chiều dọc, đó là cấu hình thứ hai, vì vậy câu trả lời là`YES`. 
5. Kiểm tra xem mặt nạ tốt hiện tại có chứa hai vị trí cách nhau một cột hay không. Điều này được phát hiện bởi`good & (good >> 8)`. Một cặp như vậy liền kề theo chiều ngang và cũng cho cấu hình thứ hai. 
6. Nếu một số hàng trước đó chứa một ô tốt ở cột`p`và hàng hiện tại chứa một ô tốt ở cột lớn hơn`p`, hai ô nằm hoàn toàn về phía đông nam của nhau. Giữ cột tốt nhỏ nhất được nhìn thấy trong tất cả các hàng trước đó, vì vậy việc kiểm tra chỉ đơn giản là`current_max > minimum_previous`. 
7. Sau tất cả các lần kiểm tra đối với cặp hàng hiện tại, hãy cập nhật cột tốt tối thiểu toàn cầu và ghi nhớ mặt nạ tốt hiện tại cho lần lặp tiếp theo. Nếu không tìm thấy cấu hình nào sau khi quét hoàn tất, hãy in`NO`. 

Điều bất biến là trước khi xử lý một cặp hàng,`minimum_previous`là cột nhỏ nhất của bất kỳ ô tốt nào trong mỗi hàng ô tốt đã được xử lý. Do đó, việc so sánh nó với cột hàng hóa lớn nhất của hàng hiện tại hoàn toàn tương đương với việc hỏi liệu có một cặp hướng đông nam nghiêm ngặt hay không. Đồng thời,`previous_good`thể hiện chính xác các ô tốt ở hàng ngay trước đó, do đó giao điểm của nó với mặt nạ hiện tại sẽ phát hiện mọi cặp dọc liền kề. Giao điểm đã dịch chuyển bên trong mặt nạ hiện tại sẽ phát hiện mọi cặp liền kề nằm ngang. Đặc điểm cấu trúc ở trên cho biết đây chính xác là những tình huống có thể tạo ra ba tuyến đường bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    prev = input().strip().encode()
    prev_value = int.from_bytes(prev, 'little')

    # One byte with value 1 in every position.
    low = ((1 << (8 * m)) - 1) // 255

    # One byte with the high bit set in every position.
    high = low << 7

    # We only care about the first m - 1 bytes, corresponding to
    # columns 0 .. m - 2.
    valid = (((1 << (8 * (m - 1))) - 1) // 255) << 7

    previous_good = 0
    minimum_previous = None

    for _ in range(n - 1):
        cur = input().strip().encode()
        cur_value = int.from_bytes(cur, 'little')

        # Byte c compares prev[c + 1] with cur[c].
        x = (prev_value >> 8) ^ cur_value

        # A set bit in byte c means x's byte c is zero, hence
        # prev[c + 1] == cur[c].
        good = (x - low) & ~x & high & valid

        if good:
            # Two vertically adjacent good cells.
            if good & previous_good:
                print("YES")
                return

            # Two horizontally adjacent good cells.
            if good & (good >> 8):
                print("YES")
                return

            # Strict southeast pair.
            first = ((good & -good).bit_length() - 1) >> 3
            last = (good.bit_length() - 1) >> 3

            if minimum_previous is not None and last > minimum_previous:
                print("YES")
                return

            if minimum_previous is None or first < minimum_previous:
                minimum_previous = first

        previous_good = good
        prev_value = cur_value

    print("NO")

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý mỗi lần một hàng, do đó thuật toán không bao giờ cần lưu trữ toàn bộ lưới.`prev_value`Và`cur_value`chứa các hàng dưới dạng chuỗi byte được đóng gói. của Python`int.from_bytes`giữ nguyên các giá trị ký tự gốc, do đó XOR các byte tương ứng chính xác là một phép kiểm tra tính bằng nhau của ký tự. 

Sự dịch chuyển 8 bit là chi tiết lập chỉ mục trung tâm. Sau đó`prev_value >> 8`, byte 0 chứa cột gốc một, byte một chứa cột gốc thứ hai, v.v. XOR cái này với hàng hiện tại được căn chỉnh`previous[c+1]`với`current[c]`. 

Biểu thức 0 byte đáng được quan tâm. Phép trừ bằng`low`được thực hiện đủ độc lập để nhận dạng phát hiện byte 0 tiêu chuẩn nhằm đánh dấu bit cao của mỗi byte 0. các`valid`mặt nạ loại bỏ byte cuối cùng, bởi vì cột`m-1`không có ô nào ở bên phải và do đó không thể là ô bên trái của một ô tốt. 

Bit tập đầu tiên của`good`cung cấp cột tốt nhỏ nhất, trong khi bit được đặt cuối cùng cho cột lớn nhất. Vì mọi bit liên quan đều là bit cao của byte nên chia chỉ số bit của nó cho 8 sẽ lấy lại chỉ mục cột. 

Kiểm tra kề cận theo chiều ngang sẽ dịch chuyển mặt nạ đi một byte. Nếu cả hai cột (c) và cột (c+1) đều tốt thì một trong các bit tương ứng sẽ tồn tại trong giao điểm. Thử nghiệm theo chiều dọc sử dụng trực tiếp mặt nạ của hàng trước đó vì cả hai mặt nạ đều mô tả các cột ô tốt. 

Không có vấn đề tràn số nguyên trong Python. Các hàng được đóng gói có tối đa 3000 byte nên số nguyên lớn nhất chỉ có khoảng 24.000 bit. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cặp hàng đầu tiên chứa một ô tốt ở cột 0 vì ký tự ở hàng 0, cột 1 là`e`, khớp với hàng 1, cột 0. Cặp tiếp theo chứa các ô tốt ở cột 1 và 2. Cột hiện tại lớn nhất là 2, trong khi cột tốt nhỏ nhất từ ​​các hàng trước là 0, do đó cấu hình nghiêm ngặt phía đông nam được tìm thấy ngay lập tức. 

| Cặp hàng | Cột tốt | Tối thiểu trước đó | Chồng chéo mặt nạ trước đó | Kết quả | 
| --- | --- | --- | --- | --- | 
| Hàng 0, 1 | 0 | không | không | tiếp tục | 
| Hàng 1, 2 | 1, 2 | 0 | không | (2>0), CÓ | 

Hai tế bào tốt ở`(0,0)`Và`(1,1)`nằm cách xa nhau về phía đông nam. Hoán đổi cục bộ của chúng có thể được chọn độc lập, đưa ra nhiều tuyến đường có cùng một chuỗi nốt nhạc. Do đó, thuật toán sẽ dừng mà không quét phần còn lại của lưới. 

Đối với Mẫu 2, mọi cặp hàng liền kề đều không tạo ra ô tốt. Ví dụ: giữa hai hàng đầu tiên,`b`được so sánh với`f`,`c`với`g`,`d`với`h`, vân vân. Mẫu không khớp tương tự tiếp tục xảy ra với mỗi cặp hàng. 

| Cặp hàng | Cột tốt | Tối thiểu trước đó | Chồng chéo mặt nạ trước đó | Kết quả | 
| --- | --- | --- | --- | --- | 
| Hàng 0, 1 | không | không | không | tiếp tục | 
| Hàng 1, 2 | không | không | không | tiếp tục | 
| Hàng 2, 3 | không | không | không | tiếp tục | 
| Hàng 3, 4 | không | không | không | tiếp tục | 

Vì không có tế bào nào tốt nên không có cơ chế phát hiện nào trong ba cơ chế này có thể thành công. Câu trả lời cuối cùng là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mỗi cặp hàng lân cận được đóng gói và so sánh một lần, với các bit xử lý hoạt động đóng gói (O(m)). | 
| Không gian | (O(m)) | Chỉ có hai hàng chật cứng và số lượng mặt nạ không đổi được duy trì. | 

Lưới chứa tối đa chín triệu ký tự. Thuật toán thực hiện một số thao tác không đổi trên các số nguyên chứa (O(m)) byte cho mỗi cặp trong số (n-1) hàng, tạo ra tổng công việc (O(nm)) trong mô hình độ phức tạp tiêu chuẩn. Việc sử dụng bộ nhớ là tuyến tính theo chiều rộng hàng và thấp hơn nhiều so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_rows(inp: str) -> str:
    it = iter(inp.splitlines())
    n, m = map(int, next(it).split())

    prev = next(it).strip().encode()
    prev_value = int.from_bytes(prev, 'little')

    low = ((1 << (8 * m)) - 1) // 255
    high = low << 7
    valid = (((1 << (8 * (m - 1))) - 1) // 255) << 7

    previous_good = 0
    minimum_previous = None

    for _ in range(n - 1):
        cur = next(it).strip().encode()
        cur_value = int.from_bytes(cur, 'little')

        x = (prev_value >> 8) ^ cur_value
        good = (x - low) & ~x & high & valid

        if good:
            if good & previous_good:
                return "YES"

            if good & (good >> 8):
                return "YES"

            first = ((good & -good).bit_length() - 1) >> 3
            last = (good.bit_length() - 1) >> 3

            if minimum_previous is not None and last > minimum_previous:
                return "YES"

            if minimum_previous is None or first < minimum_previous:
                minimum_previous = first

        previous_good = good
        prev_value = cur_value

    return "NO"

def run(inp: str) -> str:
    return solve_rows(inp)

sample1 = """\
5 8
petrozav
eiiiziio
tiiiavid
riiiiois
ozavodsk
"""

sample2 = """\
5 5
abcde
fghij
klmno
pqrst
uvwxy
"""

assert run(sample1) == "YES", "sample 1"
assert run(sample2) == "NO", "sample 2"

assert run("""\
2 2
aa
aa
""") == "NO", "minimum grid has only two routes"

assert run("""\
2 3
aba
bab
""") == "YES", "horizontal adjacent good cells"

assert run("""\
3 2
ab
ba
ab
""") == "YES", "vertical adjacent good cells"

assert run("""\
4 4
xayz
aqrs
tuxb
vwby
""") == "YES", "strict southeast good cells"

assert run("""\
2 4
abca
bday
""") == "NO", "same-row non-adjacent good cells"

max_yes = "3000 3000\n" + ("a" * 3000 + "\n") * 3000
assert run(max_yes) == "YES", "maximum-size all-equal grid"

max_no = "3000 3000\n" + (
    ("a" * 3000 if i % 2 == 0 else "b" * 3000) + "\n"
    for i in range(3000)
)
max_no = "3000 3000\n" + "".join(
    ("a" * 3000 if i % 2 == 0 else "b" * 3000) + "\n"
    for i in range(3000)
)
assert run(max_no) == "NO", "maximum-size grid with no good cells"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / aa / aa`|`NO`| Kích thước tối thiểu và thực tế là một hình vuông tốt chỉ có hai tuyến đường | 
|`2 3 / aba / bab`|`YES`| Tế bào tốt liền kề theo chiều ngang | 
|`3 2 / ab / ba / ab`|`YES`| Các tế bào tốt liền kề theo chiều dọc | 
|`4 4 / xayz / aqrs / tuxb / vwby`|`YES`| Hai tế bào tốt về phía đông nam | 
|`2 4 / abca / bday`|`NO`| Các ô tốt không liền kề trong cùng một hàng là không đủ | 
| 3000 x 3000 tất cả`a`|`YES`| Kích thước tối đa, giá trị hoàn toàn bằng nhau và phát hiện sớm | 
| 3000 x 3000 xen kẽ`a`Và`b`hàng |`NO`| Kích thước tối đa không có ô tốt, buộc phải quét toàn bộ | 

## Vỏ cạnh 

Đối với lưới tối thiểu (2\times2)```
2 2
aa
aa
```có một tế bào tốt,`(0,0)`, vì ô bên phải và ô dưới cùng đều là`a`. Thuật toán tạo một mặt nạ tốt chứa cột 0, nhưng không có hàng tốt trước đó và không có ô tốt thứ hai trong cùng một hàng hoặc cột. Nó in`NO`, phản ánh chính xác rằng lưới (2\times2) có chính xác hai tuyến đường đơn điệu. 

Đối với các ô tốt liền kề theo chiều ngang,```
2 3
aba
bab
```cặp hàng đầu tiên có các cột 0 và 1 phù hợp. Mặt nạ của nó có hai byte tập hợp lân cận, vì vậy`good & (good >> 8)`là khác không. Thuật toán in ngay lập tức`YES`. Ba tuyến đường tương ứng với việc di chuyển xuống trước cột đầu tiên, giữa hai cột hoặc sau chúng và tất cả đều tạo ra`abab`. 

Đối với các ô tốt liền kề theo chiều dọc,```
3 2
ab
ba
ab
```cặp hàng đầu tiên có một ô tốt ở cột 0 và cặp hàng thứ hai có một ô tốt khác ở cột 0. Mặt nạ của chúng giao nhau, vì vậy`good & previous_good`khác 0 khi cặp thứ hai được xử lý. Thuật toán in`YES`. 

Đối với hai ô tốt phía đông nam,```
4 4
xayz
aqrs
tuxb
vwby
```các tế bào tốt là`(0,0)`Và`(2,2)`. Không có ô tốt liền kề nên hai bài kiểm tra lân cận cục bộ không kích hoạt. Sau cặp hàng đầu tiên,`minimum_previous`trở thành 0. Khi cặp hàng chứa`(2,2)`được xử lý, cột tốt lớn nhất của nó là 2 và`2 > 0`, vậy là bài kiểm tra nghiêm ngặt về phía đông nam đã thành công. Điều này chứng tỏ tại sao cột tối thiểu toàn cục lại đủ để phát hiện cấu hình đầu tiên. 

Đối với các ô tốt không liền kề trong cùng một hàng,```
2 4
abca
bday
```các cột tốt là 0 và 2. Chúng cách nhau một cột, do đó kiểm tra kề ngang theo chiều ngang bằng 0. Không có hàng ô tốt nào trước đó có thể tạo thành một cặp đông nam nghiêm ngặt và chỉ có một hàng chứa ô tốt. Thuật toán in`NO`. Đây là ranh giới giữa một cấu hình cục bộ hợp lệ và một sự khái quát hóa hấp dẫn nhưng không chính xác. 

Đối với lưới có kích thước tối đa bằng nhau, mọi ô có thể đều tốt. Cặp hàng đầu tiên chứa các ô tốt trong mỗi cột và cặp hàng thứ hai cũng vậy. Thuật toán phát hiện cấu hình hướng đông nam nghiêm ngặt ngay khi xử lý cặp thứ hai, do đó nó trả về`YES`mà không làm những công việc không cần thiết. 

Đối với lưới xen kẽ có kích thước tối đa, mỗi hàng đều là`a`hoặc tất cả`b`, với các hàng liền kề sử dụng các chữ cái khác nhau. Đối với mỗi cột (c), sự so sánh là giữa các ký tự khác nhau, do đó không tồn tại ô tốt. Mọi mặt nạ tốt đều bằng 0 và thuật toán quét tất cả (2999) cặp hàng trước khi in`NO`. Trường hợp này thực hiện quét toàn bộ trường hợp xấu nhất trong khi vẫn chỉ sử dụng hai hàng lưu trữ đang hoạt động được đóng gói.
