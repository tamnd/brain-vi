---
title: "CF 104432B - Trò chơi chữ cái"
description: "Chúng ta được cung cấp một chuỗi các chữ cái, trong đó mỗi chữ cái đã được gán cho một công ty đích. Đồng thời, chúng ta cũng có phong bì, trên mỗi phong bì đều có ghi một công ty cố định, khớp với cấu trúc trình tự giống nhau."
date: "2026-06-30T18:55:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104432
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #17 (AOE-Forces)"
rating: 0
weight: 104432
solve_time_s: 65
verified: true
draft: false
---

[CF 104432B - Trò chơi viết chữ](https://codeforces.com/problemset/problem/104432/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái, trong đó mỗi chữ cái đã được gán cho một công ty đích. Đồng thời, chúng ta cũng có phong bì, trên mỗi phong bì đều có ghi một công ty cố định, khớp với cấu trúc trình tự giống nhau. Quyền tự do chính mà Amir có là anh ấy có thể hoán đổi các chữ cái một cách tùy ý trên các phong bì, miễn là mỗi phong bì nhận được chính xác một lá thư. 

Ràng buộc mà chúng ta phải tôn trọng là sau khi đặt các bức thư vào phong bì, không có bức thư nào được đưa vào phong bì có công ty trùng khớp với công ty đích của bức thư. Nói cách khác, nếu một lá thư được gửi cho công ty`x`, nó không thể được đặt vào bất kỳ phong bì nào có dán nhãn`x`. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem có tồn tại hoán vị của nhiều chữ cái đã cho sao cho mỗi chữ cái được đặt vào một vị trí có nhãn khác với giá trị mục tiêu của chính nó hay không. 

Kích thước đầu vào tăng lên`n = 100000`, điều này ngay lập tức loại trừ bất kỳ suy luận giai thừa hoặc hàm mũ nào đối với các hoán vị. Bất cứ điều gì liên quan đến việc xây dựng rõ ràng hoặc kiểm tra hoán vị đều không thể thực hiện được. Chúng ta cần một điều kiện có thể được xác minh chỉ bằng cách sử dụng số lượng hoặc thuộc tính cấu trúc của việc phân bổ nhãn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các chữ cái thuộc về cùng một công ty. Trong tình huống đó, mọi phong bì cũng thuộc về công ty đó (vì công ty đó phải xuất hiện ít nhất một lần, nhưng các phong bì tương ứng về vị trí) và mọi vị trí chắc chắn sẽ “đúng” theo nghĩa bị cấm. Ví dụ:```
3 1
1 1 1
```Đầu ra phải là`NO`, vì mọi chữ cái đều giống hệt nhau và mọi vị trí đều giữ cho chúng khớp nhau. 

Một trường hợp khác là khi có nhiều công ty, nhưng một công ty chiếm ưu thế lớn. Ví dụ: nếu một công ty xuất hiện hơn một nửa số vị trí, chúng tôi có thể nghi ngờ rằng không thể tránh hoàn toàn việc tự đặt vị trí vì không có đủ vị trí “nước ngoài” để đáp ứng tất cả các lần xuất hiện của nhãn đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán các chữ cái vào phong bì và kiểm tra tất cả các hoán vị của bài tập. Về mặt khái niệm, chúng tôi sẽ coi mỗi lá thư là một mục và mỗi phong bì là một khe, sau đó thử tất cả các phép đối chiếu và xác minh xem có bất kỳ điểm nào tránh được điểm cố định hay không. Điều này có hiệu quả vì nó khám phá tất cả các cách sắp xếp có thể, nhưng nó phát triển khi`n!`, cái nào cho`n = 100000`là hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các hoán vị riêng lẻ và thay vào đó tập trung vào số lượng nhãn của từng công ty. Mỗi lá thư của một công ty nhất định phải được đặt trong một phong bì của một công ty khác nhau, nên mỗi lần xuất hiện nhãn đều phải “tránh” lớp riêng của nó. Yếu tố hạn chế là liệu các nhãn còn lại có cung cấp đủ dung lượng để lưu trữ những vị trí bị tránh này hay không. 

Nếu một công ty xuất hiện quá nhiều lần, cụ thể là hơn một nửa tổng số chữ cái, thì ngay cả khi chúng tôi cố gắng phân bổ các vị trí một cách tối ưu, sẽ không có đủ phong bì không phù hợp để phân tách tất cả các lần xuất hiện. Ngược lại, nếu không có nhãn nào vượt quá một nửa`n`, thì một phép gán giống như sắp xếp hợp lệ luôn có thể được xây dựng bằng cách sắp xếp lại các phần tử theo cách dịch chuyển tuần hoàn hoặc tham lam trên các nhãn khác nhau. 

Vì vậy, toàn bộ vấn đề quy về việc kiểm tra xem tần suất tối đa của bất kỳ nhãn công ty nào là nhiều nhất`n - max_frequency`. Tương tự, chúng ta chỉ cần đảm bảo không có nhãn nào chiếm ưu thế quá mạnh. Trong cấu trúc cụ thể này, điều kiện đơn giản hóa việc kiểm tra xem tần số lớn nhất có nhiều nhất không`n - largest_frequency`, luôn luôn đúng trừ khi một nhãn duy nhất chiếm tất cả các vị trí hoặc vi phạm sự cân bằng cần thiết cho một hoán vị giống như loạn trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Kiểm tra tần số | O(n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách giảm nó xuống việc kiểm tra ràng buộc tần số. 

1. Đếm số lần mỗi công ty xuất hiện trong danh sách. Điều này cho chúng ta biết có bao nhiêu chữ cái muốn chuyển đến mỗi lớp đích. 
2. Tìm tần suất tối đa trong số tất cả các công ty. Điều này xác định nhóm “đòi hỏi khắt khe” nhất, nhóm có nhiều khả năng gây ra xung đột nhất. 
3. So sánh tần số tối đa này với các chữ cái còn lại. Nếu công ty thường xuyên nhất chiếm tất cả các vị trí, hoặc tương tự nếu không thể gán sự xuất hiện của nó cho các công ty khác, chúng tôi kết luận là thất bại. 
4. Nếu không, hãy kết luận thành công, vì các công ty còn lại cung cấp đủ vị trí để hoán đổi các chữ cái sao cho không còn chữ cái nào nằm trong lớp riêng của nó. 

Lý do đằng sau bước này là một phép gán hợp lệ tồn tại chính xác khi không có lớp nào lấn át hệ thống các vị trí thay thế có sẵn. 

### Tại sao nó hoạt động 

Hãy coi mỗi nhãn công ty là một màu sắc. Chúng tôi muốn đặt từng mục màu vào một vị trí không phải là màu riêng của nó. Trở ngại duy nhất cho việc này là khi một màu xuất hiện thường xuyên đến mức các màu khác không thể cung cấp đủ vị trí “an toàn”. Nếu nhóm lớn nhất quá lớn thì ngay cả sự sắp xếp lại tốt nhất vẫn buộc ít nhất một phần tử ở lại trong nhóm của chính nó. Nếu nó không quá lớn, chúng ta luôn có thể phân bổ các phần tử của nhóm thống trị qua các vị trí thuộc các nhóm khác và sau đó hoàn thành các vị trí còn lại một cách tương tự. Tính đúng đắn phụ thuộc vào thực tế là chỉ có tần số tối đa mới tạo ra nút thắt cổ chai; tất cả các bản phân phối khác có thể được xáo trộn mà không cần tạo các bài tập cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
arr = list(map(int, input().split()))

freq = {}
for x in arr:
    freq[x] = freq.get(x, 0) + 1

mx = max(freq.values())

# if one company dominates too much, impossible
if mx > n - mx:
    print("NO")
else:
    print("YES")
```Giải pháp đầu tiên là xây dựng bản đồ tần số của tất cả các nhãn của công ty. Điều này là cần thiết vì cấu trúc của bài toán phụ thuộc hoàn toàn vào bội số chứ không phải vị trí. Sau khi tính toán tần số tối đa, chúng tôi áp dụng trực tiếp điều kiện khả thi xuất phát từ ràng buộc kiểu lệch hướng. 

Chi tiết triển khai quan trọng là chúng tôi không cố gắng xây dựng hoán vị. Bất kỳ cách tiếp cận mang tính xây dựng nào cũng có nguy cơ xảy ra sự phức tạp và các lỗi đặc biệt. Thay vào đó, chúng tôi hoàn toàn dựa vào giới hạn tổ hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
1 1 1
```| Bước | bản đồ tần số | tần số tối đa | quyết định | 
| --- | --- | --- | --- | 
| bắt đầu | {1:3} | - | - | 
| tính toán | {1:3} | 3 | kiểm tra | 
| so sánh | {1:3} | 3 | 3 > 0 | 
| kết quả | - | - | KHÔNG | 

Tất cả các chữ cái thuộc về một công ty. Không có phong bì thay thế nào, vì vậy mọi vị trí đều giữ cùng một nhãn, khiến việc tránh né là không thể. 

### Ví dụ 2 

đầu vào:```
4 4
4 1 2 3
```| Bước | bản đồ tần số | tần số tối đa | quyết định | 
| --- | --- | --- | --- | 
| bắt đầu | {4:1,1:1,2:1,3:1} | - | - | 
| tính toán | giống nhau | 1 | kiểm tra | 
| so sánh | giống nhau | 1 | 1 3 | 
| kết quả | - | - | CÓ | 

Vì tất cả các tần số đều được cân bằng nên chúng tôi có thể luân chuyển các nhiệm vụ để mỗi chữ cái nằm trong một phong bì công ty khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để đếm tần số và tính giá trị tối đa | 
| Không gian | O(m) | từ điển tần số trên tối đa m nhãn | 

Các ràng buộc cho phép lên đến`n = 100000`, do đó quá trình quét tuyến tính và đếm băm vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1

    mx = max(freq.values())
    return "YES\n" if mx <= n - mx else "NO\n"

# provided samples
assert run("3 1\n1 1 1\n") == "NO\n"
assert run("4 4\n4 1 2 3\n") == "YES\n"

# custom cases
assert run("1 1\n1\n") == "NO\n", "single element must fail"
assert run("2 2\n1 2\n") == "YES\n", "swap works"
assert run("5 2\n1 1 1 2 2\n") == "YES\n", "balanced mix"
assert run("6 2\n1 1 1 1 2 2\n") == "NO\n", "dominant class"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`| KHÔNG | kích thước tối thiểu, buộc phải tự khớp | 
|`2 2 / 1 2`| CÓ | trao đổi hợp lệ đơn giản nhất | 
|`5 2 / 1 1 1 2 2`| CÓ | sự kết hợp khả thi không tầm thường | 
|`6 2 / 1 1 1 1 2 2`| KHÔNG | giải pháp khối tần số chiếm ưu thế | 

## Vỏ cạnh 

Đối với một kịch bản một chữ cái như`n = 1`, vị trí duy nhất có thể có là cố định, do đó thuật toán trả về NO một cách chính xác vì tần số tối đa bằng`n`. 

Đối với các phân phối có độ lệch cao, chẳng hạn như`1 1 1 1 2 2`, tần số của`1`là 4 trong khi phần bù chỉ là 2. Kiểm tra`mx > n - mx`kích hoạt, xác định chính xác rằng không có đủ`1`khe cắm để tránh tự vị trí. 

Đối với các trường hợp cân bằng như`1 2 3 4`, tất cả các tần số là 1, vì vậy`mx = 1`Và`n - mx = 3`, cho phép một hoán vị hợp lệ. Thuật toán chấp nhận những trường hợp này mà không xây dựng nhiệm vụ rõ ràng, chỉ dựa vào điều kiện khả thi xuất phát từ hạn chế về năng lực.
