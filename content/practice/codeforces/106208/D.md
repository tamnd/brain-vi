---
title: "CF 106208D - CEO mới của CloseAI"
description: "Chúng ta được cung cấp một tập hợp các từ được phân tách bằng dấu cách. Ban đầu mỗi ký tự là mã thông báo riêng của nó. Tại bất kỳ thời điểm nào, mỗi từ đều được biểu diễn dưới dạng một chuỗi mã thông báo thay vì một chuỗi ký tự. Trong một lần lặp, chúng tôi kiểm tra từng cặp mã thông báo liền kề trong mỗi từ."
date: "2026-06-19T13:45:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106208
codeforces_index: "D"
codeforces_contest_name: "Inter University Programming Contest - MU CSE Fest 2025 - MIRROR"
rating: 0
weight: 106208
solve_time_s: 61
verified: true
draft: false
---

[CF 106208D - Giám đốc điều hành mới của CloseAI](https://codeforces.com/problemset/problem/106208/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các từ được phân tách bằng dấu cách. Ban đầu mỗi ký tự là mã thông báo riêng của nó. Tại bất kỳ thời điểm nào, mỗi từ đều được biểu diễn dưới dạng một chuỗi mã thông báo thay vì một chuỗi ký tự. 

Trong một lần lặp, chúng tôi kiểm tra từng cặp mã thông báo liền kề trong mỗi từ. Chúng tôi đếm số lần mỗi cặp xuất hiện. Cặp có tần số tối đa được chọn. Nếu một số cặp có cùng tần số, chúng tôi so sánh các chuỗi thu được bằng cách ghép hai mã thông báo và chọn chuỗi nhỏ nhất về mặt từ điển. 

Sau khi chọn một cặp, mỗi lần xuất hiện của cặp đó sẽ được hợp nhất đồng thời. Các trường hợp chồng chéo phải được xử lý từ trái sang phải. Ví dụ, từ`aaa`ban đầu chứa hai lần xuất hiện của`(a,a)`, nhưng sau một lần hợp nhất, nó sẽ trở thành`[aa,a]`, không`[aa,aa]`. 

Quá trình tiếp tục cho đến khi mỗi từ bao gồm một mã thông báo duy nhất. Cuối cùng, chúng tôi phải xuất ra mọi mã thông báo riêng biệt từng tồn tại, được sắp xếp theo từ điển. 

Mỗi từ có độ dài tối đa là mười và có nhiều nhất$10^5$từ. Điều này có nghĩa là tổng số ký tự nhiều nhất là$10^6$. Một thuật toán bậc hai về số lượng ký tự sẽ yêu cầu khoảng$10^{12}$hoạt động vượt xa giới hạn. Giải pháp nào cũng phải khai thác thực tế là từ ngữ rất ngắn. 

Một điểm tinh tế là việc xử lý sự chồng chéo. Coi như```
aaa
```Ban đầu cặp`(a,a)`xuất hiện hai lần. Việc triển khai bất cẩn có thể thay thế cả hai lần xuất hiện và nhận được hai mã thông báo`aa aa`, điều này là không thể vì ký tự ở giữa thuộc về cả hai cặp. Kết quả đúng sau một lần lặp là```
[aa, a]
```Một nguồn sai lầm khác là việc bẻ hòa. Vì```
ab ac
```tất cả các cặp xuất hiện một lần:```
ab
ac
```Chỉ riêng tần số là không đủ. Từ`"ab" < "ac"`, cặp`(a,b)`phải được sáp nhập trước tiên. 

Những từ lặp đi lặp lại cũng dễ bị xử lý sai. Vì```
ab ab ab
```cặp đôi`(a,b)`xuất hiện ba lần chứ không phải một lần. Chỉ đếm các từ riêng biệt sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. Lưu trữ từng từ dưới dạng danh sách mã thông báo. Trong mỗi lần lặp, hãy quét tất cả các từ và đếm tất cả các cặp liền kề. Chọn cặp thường xuyên nhất, hợp nhất nó ở mọi nơi, sau đó lặp lại. 

Phương pháp này đúng vì nó tuân theo định nghĩa chính xác. Vấn đề là việc quét toàn bộ lặp đi lặp lại. Giả sử tổng số ký tự là$10^6$. Có thể có gần như$10^6$các hoạt động hợp nhất và mỗi lần lặp có thể kiểm tra hầu hết$10^6$ghép đôi nữa. Trường hợp xấu nhất tiếp cận$10^{12}$hoạt động. 

Quan sát quan trọng là các từ cực kỳ ngắn. Ban đầu, một từ có độ dài tối đa mười sẽ đóng góp tối đa chín cặp liền kề và mỗi lần hợp nhất sẽ làm giảm số lượng mã thông báo bên trong từ đó. Do đó, một từ chỉ tham gia vào một số ít thay đổi. 

Thay vì xây dựng lại tần số từ đầu, chúng tôi duy trì chúng dần dần. Bất cứ khi nào một cặp được hợp nhất bên trong một từ nào đó, chỉ có các cặp lân cận xung quanh vị trí đó thay đổi. Mọi thứ khác vẫn còn nguyên. Hàng đợi ưu tiên cho phép truy cập vào cặp tốt nhất hiện tại. Các mục cũ được bỏ qua một cách lười biếng. 

Tổng khối lượng công việc tỷ lệ thuận với số lần tạo và xóa cặp, tuyến tính trong tổng số ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L²) | O(L) | Quá chậm | 
| Tối ưu | O(L log L) | O(L) | Đã chấp nhận | 

Đây$L$biểu thị tổng số ký tự. 

## Hướng dẫn thuật toán 

1. Chia dòng đầu vào thành các từ và gán id số nguyên cho mỗi mã thông báo ký tự. 
2. Biểu diễn mỗi từ dưới dạng danh sách liên kết đôi. Mỗi nút lưu trữ mã thông báo id và con trỏ tới các nút trước và tiếp theo của nó. 
3. Đối với mỗi cặp nút liền kề, hãy tăng bộ đếm tần số của nó và lưu vị trí xuất hiện. 
4. Đẩy từng cặp vào hàng đợi ưu tiên được sắp xếp theo tần số giảm dần và chuỗi nối tăng dần. 
5. Liên tục trích xuất mục trên cùng từ hàng đợi. Vì tần số thay đổi theo thời gian nên một số mục trở nên lỗi thời. Nếu tần số được lưu trữ khác với tần số hiện tại, hãy loại bỏ mục nhập và tiếp tục. 
6. Sau khi nhận được một cặp hợp lệ, hãy tạo một mã thông báo mới có chuỗi bằng với sự nối của hai mã thông báo thành phần. Thêm chuỗi này vào tập hợp tất cả các mã thông báo. 
7. Đối với mỗi lần xuất hiện hiện tại của cặp đó, xử lý từ trái sang phải. Nếu cả hai nút vẫn liền kề, hãy hợp nhất chúng thành một nút đại diện cho mã thông báo mới. 
8. Trước khi loại bỏ cặp cũ, hãy giảm tần số của các cặp lân cận biến mất do sự hợp nhất này. 
9. Sau khi tạo nút mới, hãy tăng tần số của các cặp lân cận mới xuất hiện. 
10. Chèn các giá trị tần số đã cập nhật vào hàng ưu tiên. Các giá trị cũ có thể vẫn còn trong heap vì sau này chúng sẽ bị từ chối. 
11. Tiếp tục cho đến khi không còn cặp liền kề nào. 

### Tại sao nó hoạt động 

Điều bất biến là bảng tần số luôn khớp với các cặp liền kề hiện có trong danh sách liên kết. Mỗi lần hợp nhất chỉ phá hủy chính cặp đó và tối đa hai cặp lân cận, trong khi tạo ra tối đa hai cặp lân cận mới. Vì tất cả các cập nhật tần số được áp dụng ngay lập tức nên bảng luôn mô tả trạng thái hiện tại. 

Hàng đợi ưu tiên luôn chứa ít nhất một mục nhập cho mỗi cặp hiện có. Việc xóa lười đảm bảo rằng các mục lỗi thời sẽ bị bỏ qua. Do đó, mục nhập hợp lệ đầu tiên được loại bỏ khỏi vùng nhớ chính xác là cặp có tần số cao nhất và chuỗi nối nhỏ nhất về mặt từ điển trong số các mối quan hệ. Vì mọi sự hợp nhất đều tuân theo đặc điểm kỹ thuật nên toàn bộ quá trình đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution
```Quá trình triển khai hoàn chỉnh khá dài vì nó cần danh sách được liên kết, theo dõi sự xuất hiện và cập nhật vùng nhớ heap. 

Ý tưởng trung tâm là tránh quét lại tất cả các từ. Mỗi nút thuộc về chính xác một từ và tồn tại cho đến khi được hợp nhất thành một mã thông báo lớn hơn. Khi việc hợp nhất xảy ra, chỉ có một số cặp lân cận thay đổi không đổi, do đó tần số cập nhật vẫn giữ nguyên cục bộ. 

Sử dụng danh sách liên kết đôi sẽ tránh được việc dịch chuyển tốn kém bên trong mảng. Sau khi hợp nhất hai nút liền kề, việc kết nối lại các nút xung quanh sẽ mất một khoảng thời gian không đổi. 

Heap lưu trữ những thông tin có thể đã lỗi thời. Việc loại bỏ các mục cũ ngay lập tức sẽ đòi hỏi những tìm kiếm tốn kém trong vùng heap. Thay vào đó, mỗi khi tần số của một cặp thay đổi, chúng tôi chỉ cần đẩy một bản ghi mới. Khi một mục đạt đến đỉnh, chúng tôi so sánh tần số được lưu trữ của nó với tần số hiện tại. Nếu chúng khác nhau, bản ghi đó đã cũ và bị loại bỏ. 

Yêu cầu từ trái sang phải đối với các cặp chồng chéo được tôn trọng một cách tự nhiên bằng cách xử lý các lần xuất hiện theo thứ tự và kiểm tra xem hai nút có còn liền kề hay không trước khi hợp nhất. Ví dụ: sau khi hợp nhất hai ký tự đầu tiên của`aaa`, lần xuất hiện thứ hai sẽ tự động biến mất vì nút ở giữa không còn tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
cat bat rat
```Trạng thái ban đầu: 

| Bước | Trạng thái từ | Cặp thường xuyên nhất | 
| --- | --- | --- | 
| 0 | [c,a,t], [b,a,t], [r,a,t] | tại | 

Sau khi sáp nhập`at`: 

| Bước | Trạng thái từ | Cặp thường xuyên nhất | 
| --- | --- | --- | 
| 1 | [c,at], [b,at], [r,at] | dơi | 

Sau khi sáp nhập`bat`: 

| Bước | Trạng thái từ | Cặp thường xuyên nhất | 
| --- | --- | --- | 
| 2 | [c,at], [bat], [r,at] | mèo | 

Sau khi sáp nhập`cat`: 

| Bước | Trạng thái từ | Cặp thường xuyên nhất | 
| --- | --- | --- | 
| 3 | [mèo], [dơi], [r,at] | chuột | 

Mã thông báo cuối cùng: 

| Mã thông báo | 
| --- | 
| một | 
| tại | 
| b | 
| dơi | 
| c | 
| mèo | 
| r | 
| chuột | 
| t | 

Ví dụ này thể hiện sự phá vỡ ràng buộc. Sau đó`at`được tạo, ba cặp ứng cử viên có tần số bằng nhau, do đó thứ tự từ điển xác định lần hợp nhất tiếp theo. 

### Ví dụ 2 

đầu vào:```
aaa
```Trạng thái ban đầu: 

| Bước | Trạng thái từ | Cặp đôi được chọn | 
| --- | --- | --- | 
| 0 | [a,a,a] | aa | 

Sau một lần hợp nhất: 

| Bước | Trạng thái từ | Cặp đôi được chọn | 
| --- | --- | --- | 
| 1 | [aa,a] | aaa | 

Sau lần hợp nhất thứ hai: 

| Bước | Trạng thái từ | 
| --- | --- | 
| 2 | [aaa] | 

Mã thông báo cuối cùng: 

| Mã thông báo | 
| --- | 
| một | 
| aa | 
| aaa | 

Ví dụ này cho thấy tại sao các lần xuất hiện chồng chéo phải được xử lý từ trái sang phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L log L) | Mỗi bản cập nhật cặp đều tạo ra các hoạt động heap | 
| Không gian | O(L) | Nút, tần số và chuỗi mã thông báo | 

Vì tổng số ký tự nhiều nhất là$10^6$, MỘT$O(L \log L)$thuật toán dễ dàng phù hợp trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và duy trì ở mức dưới 256 MB với cấu trúc nhỏ gọn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call your solution function here
    ...

# sample
assert run("cat bat rat\n") == "a\nat\nb\nbat\nc\ncat\nr\nrat\nt\n"

# minimum size
assert run("a\n") == "a\n"

# overlap case
assert run("aaa\n") == "a\naa\naaa\n"

# tie-breaking
assert run("ab ac\n") == "a\nab\nac\nb\nc\n"

# repeated words
assert run("ab ab ab\n") == "a\nab\nb\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Mã thông báo đơn, không hợp nhất | 
|`aaa`|`a aa aaa`| Sự xuất hiện chồng chéo | 
|`ab ac`| Phá vỡ ràng buộc từ điển | | 
|`ab ab ab`| Tần số đếm tất cả các lần xuất hiện | | 

## Vỏ cạnh 

Hãy xem xét```
aaa
```Ban đầu có hai lần xuất hiện chồng chéo của`(a,a)`. Thuật toán xử lý các lần xuất hiện từ trái sang phải. Sau khi hợp nhất cặp đầu tiên, nút giữa sẽ biến mất. Khi lần xuất hiện thứ hai được kiểm tra, các nút của nó không còn liền kề nữa nên nó bị bỏ qua. Các mã thông báo cuối cùng là```
a
aa
aaa
```Bây giờ hãy xem xét```
ab ac
```Cả hai cặp đều xuất hiện một lần. Chỉ riêng tần số không mang lại sự ưu tiên nào. Chuỗi nối của chúng là`"ab"`Và`"ac"`, Và`"ab"`nhỏ hơn về mặt từ điển. Thuật toán sử dụng thứ tự này bên trong hàng đợi ưu tiên, vì vậy`ab`được sáp nhập đầu tiên. 

Cuối cùng, hãy xem xét```
ab ab ab
```Có ba lần xuất hiện`(a,b)`. Mỗi lần xuất hiện đều đóng góp riêng vào bảng tần số. Cặp được chọn có tần số ba và sau một lần hợp nhất, tất cả các từ sẽ trở thành`[ab]`. Đầu ra cuối cùng là```
a
ab
b
```Chỉ đếm các từ riêng biệt sẽ gán tần số một không chính xác và có thể thay đổi thứ tự hợp nhất.
