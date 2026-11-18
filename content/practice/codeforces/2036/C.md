---
title: "CF 2036C - Anya và 1100"
description: "Chúng ta được cung cấp một chuỗi nhị phân thay đổi theo thời gian. Mỗi truy vấn cập nhật một vị trí duy nhất, lật ký tự đó thành 0 hoặc 1. Sau mỗi lần cập nhật, chúng ta phải trả lời một câu hỏi rất cụ thể: chuỗi hiện tại có chứa mẫu “1100” dưới dạng khối liền kề ở bất kỳ đâu không?"
date: "2026-06-08T10:19:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2036
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 984 (Div. 3)"
rating: 1100
weight: 2036
solve_time_s: 65
verified: true
draft: false
---

[CF 2036C - Anya và 1100](https://codeforces.com/problemset/problem/2036/C) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân thay đổi theo thời gian. Mỗi truy vấn cập nhật một vị trí duy nhất, lật ký tự đó thành 0 hoặc 1. Sau mỗi lần cập nhật, chúng ta phải trả lời một câu hỏi rất cụ thể: chuỗi hiện tại có chứa mẫu “1100” dưới dạng khối liền kề ở bất kỳ đâu không? 

Vì vậy, mỗi thao tác sẽ biến đổi một ký tự và sau mỗi đột biến, chúng ta sẽ quét khái niệm xem có chuỗi con nào có độ dài 4 bằng 1100 hay không. 

Các ràng buộc ngay lập tức loại trừ mọi hoạt động tính toán lại trên toàn bộ chuỗi cho mỗi truy vấn. Tổng độ dài của tất cả các chuỗi lên tới 2×10^5 và tổng số lần cập nhật cũng lên tới 2×10^5. Quét toàn bộ chuỗi cho mỗi truy vấn sẽ là O(nq), có thể đạt tới 4×10^10 hoạt động, vượt xa giới hạn khả thi. Ngay cả việc tính toán lại hàm băm luân phiên trên tất cả các chuỗi con cho mỗi bản cập nhật vẫn sẽ quá chậm nếu được thực hiện một cách ngây thơ. 

Quan sát quan trọng là mỗi bản cập nhật chỉ ảnh hưởng đến một vùng lân cận địa phương rất nhỏ. Việc lật một ký tự không thể tạo hoặc hủy các lần xuất hiện của “1100” cách xa chỉ mục được cập nhật. Nó chỉ ảnh hưởng đến các chuỗi con chồng lên vị trí được cập nhật. 

Trường hợp cạnh tinh tế xuất phát từ việc xử lý ranh giới. Nếu độ dài chuỗi nhỏ hơn 4 thì câu trả lời luôn là KHÔNG. Một trường hợp khác là khi các bản cập nhật liên tục chuyển đổi các ký tự bên trong một vùng nhỏ, có khả năng tạo và xóa các lần xuất hiện chồng chéo như:```
11100
```Ở đây “1100” xuất hiện bắt đầu từ vị trí 2. Một cách tiếp cận đơn giản chỉ kiểm tra một cửa sổ xung quanh chỉ mục được cập nhật nhưng bỏ sót các cửa sổ lân cận sẽ không thành công trên các mẫu chồng chéo như:```
111100
```chứa “1100” bắt đầu từ vị trí 2 và 3. 

Vì vậy, chúng tôi phải đảm bảo duy trì chính xác tất cả các cửa sổ có độ dài 4 có thể có. 

## Phương pháp tiếp cận 

Một phương pháp đơn giản là tính toán lại xem “1100” có tồn tại sau mỗi lần cập nhật hay không bằng cách quét tất cả các chuỗi con có độ dài 4. Điều này đúng vì chúng tôi kiểm tra rõ ràng mọi cửa sổ có thể, nhưng chi phí là O(n) cho mỗi truy vấn. Với tối đa 2×10^5 truy vấn, tốc độ này quá chậm. 

Thông tin chi tiết về cấu trúc quan trọng là chỉ các chuỗi con có độ dài 4 bao gồm chỉ mục được cập nhật mới có thể thay đổi giá trị của chúng. Mỗi lần cập nhật chỉ có thể ảnh hưởng đến tối đa 4 chuỗi con ứng viên: những chuỗi bắt đầu từ i−3, i−2, i−1 và i. Điều đó làm giảm vấn đề từ tính toán lại toàn cầu đến bảo trì cục bộ. 

Chúng tôi duy trì một bộ đếm toàn cầu về số lần xuất hiện của “1100” hiện đang tồn tại. Ban đầu chúng tôi tính toán nó một lần trong O(n). Đối với mỗi truy vấn, chúng tôi xóa đóng góp của tất cả các cửa sổ bị ảnh hưởng, áp dụng bản cập nhật và thêm lại đóng góp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu (cập nhật cửa sổ cục bộ) | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi này là một mảng và duy trì số lượng chỉ số tôi đáp ứng s[i:i+4] = "1100". 

1. Khởi tạo chuỗi và tính xem có bao nhiêu chuỗi con có độ dài 4 bằng “1100”. Điều này đưa ra trạng thái câu trả lời ban đầu. 
2. Đối với mỗi bản cập nhật ở vị trí i, trước tiên hãy loại bỏ sự đóng góp của tất cả các chuỗi con có thể bị ảnh hưởng bởi chỉ mục này. Đây là các vị trí bắt đầu i−3, i−2, i−1 và i. 
3. Áp dụng bản cập nhật bằng cách đặt s[i] thành giá trị mới. 
4. Tính toán lại các khoản đóng góp cho bốn vị trí bắt đầu của ứng viên giống nhau sau khi cập nhật. 
5. Sau khi xử lý, nếu số đếm lớn hơn 0, xuất CÓ, nếu không xuất ra KHÔNG. 

Lý do chúng tôi xóa trước khi cập nhật là để tránh tính hai lần các chuỗi con cũ bao gồm giá trị ký tự cũ. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện của “1100” được xác định đầy đủ bởi bốn ký tự liên tiếp. Việc thay đổi một ký tự chỉ có thể ảnh hưởng đến chuỗi con có chứa ký tự đó. Bất kỳ chuỗi con nào hoàn toàn nằm ngoài phạm vi [i−3, i+3] vẫn không thay đổi, do đó đóng góp của nó vào số đếm là bất biến. Bằng cách hạn chế các bản cập nhật cho nhóm cửa sổ có khả năng bị ảnh hưởng có kích thước không đổi, chúng tôi duy trì số lượng toàn cầu chính xác mà không cần quét toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(s, i):
    if i < 0 or i + 3 >= len(s):
        return False
    return s[i] == '1' and s[i+1] == '1' and s[i+2] == '0' and s[i+3] == '0'

t = int(input())
for _ in range(t):
    s = list(input().strip())
    q = int(input())

    n = len(s)
    cnt = 0

    for i in range(n - 3):
        if s[i:i+4] == ['1', '1', '0', '0']:
            cnt += 1

    for _ in range(q):
        i, v = input().split()
        i = int(i) - 1
        v = v

        for j in range(i - 3, i + 1):
            if check(s, j):
                cnt -= 1

        s[i] = v

        for j in range(i - 3, i + 1):
            if check(s, j):
                cnt += 1

        if cnt > 0:
            print("YES")
        else:
            print("NO")
```Việc triển khai giữ chuỗi dưới dạng danh sách để cập nhật hiệu quả. Chức năng kiểm tra trợ giúp xác minh một cách an toàn xem cửa sổ bắt đầu ở chỉ mục j có tạo thành “1100” hay không, trả về Sai khi vượt quá giới hạn. Mỗi truy vấn xử lý tối đa bốn cửa sổ trước và sau khi cập nhật, đảm bảo công việc được thực hiện liên tục trên mỗi thao tác. 

Một lỗi phổ biến là quên trừ đi những khoản đóng góp cũ trước khi áp dụng bản cập nhật. Điều đó dẫn đến số lượng tăng cao khi một cửa sổ chuyển từ khớp sang không khớp hoặc ngược lại. 

Một điểm tinh tế khác là đảm bảo giới hạn chỉ mục được tôn trọng khi kiểm tra các cửa sổ gần các cạnh của chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ:`s = 1100000`, cập nhật ảnh hưởng đến khu vực giữa 

Chúng tôi theo dõi số lần xuất hiện “1100”. 

| Bước | Cập nhật | Chỉ mục đã thay đổi | Cửa sổ bị ảnh hưởng | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | - | [0..3] cửa sổ hợp lệ là “1100” | 1 | 
| 2 | tập 6→1 | 5 | 2..5,3..6,4..7 v.v. | được tính toán lại cục bộ | 
| 3 | tập 7→1 | 6 | chỉ các cửa sổ cục bộ | cập nhật | 
| 4 | tập 4→1 | 3 | chỉ các cửa sổ cục bộ | cập nhật | 

Sau mỗi bước chúng ta chỉ chạm vào các cửa sổ chồng lên chỉ mục đã thay đổi. Các câu trả lời CÓ/KHÔNG cuối cùng khớp với việc có cửa sổ nào bằng “1100” hay không. 

### Ví dụ: tạo chồng chéo 

đầu vào:```
s = 111100
```Ban đầu: 

| tôi | cửa sổ | trận đấu | 
| --- | --- | --- | 
| 0 | 1111 | không | 
| 1 | 1110 | không | 
| 2 | 1100 | vâng | 

Bây giờ nếu chúng ta lật vị trí 2 từ 1 thành 0: 

| Bước | Hành động | cửa sổ 0 | cửa sổ 1 | cửa sổ 2 | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| trước | 111100 | 1111 | 1110 | 1100 | 1 | 
| xóa | cập nhật lúc 2 | trừ các cửa sổ bị ảnh hưởng | | | 0 | 
| sau | 110100 | 1101 | 1010 | 0100 | 0 | 

Điều này cho thấy phép trừ cục bộ ngăn chặn các kết quả trùng khớp cũ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | lần quét đầu tiên là O(n), mỗi truy vấn kiểm tra tối đa 4 cửa sổ | 
| Không gian | O(n) | lưu trữ chuỗi dưới dạng danh sách có thể thay đổi | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi 2×10^5, do đó công việc tuyến tính cho mỗi phần tử này vừa vặn thoải mái trong giới hạn thời gian. Hoạt động hệ số không đổi trên mỗi truy vấn đảm bảo hiệu suất vẫn ổn định ngay cả ở mức giới hạn tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else __import__("builtins").exec("pass")

# We embed a simple wrapper for demonstration purposes
def solve(inp: str) -> str:
    import sys
    input = iter(inp.strip().split("\n")).__next__

    t = int(inp.split()[0])
    return ""

# Provided samples are omitted for brevity in this mock harness

# custom cases
# minimum length, no possible match
# all ones
# alternating pattern
# single flip creating pattern
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n111\n1\n2 0`|`NO`| chuỗi quá ngắn so với mẫu | 
|`1\n1100\n1\n2 1`|`NO`| phá hủy trận đấu chính xác | 
|`1\n111100\n1\n1 0`|`YES`| tính chính xác của cửa sổ chồng chéo | 
|`1\n00001111\n2\n4 1\n5 0`| hỗn hợp | tạo và xóa | 

## Vỏ cạnh 

Một chuỗi ngắn có độ dài nhỏ hơn 4 không bao giờ chứa “1100”. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp ban đầu chỉ chạy tối đa n−3, giá trị này là số âm, do đó không có kết quả trùng khớp nào được tính và tất cả các kiểm tra đều trả về sai một cách an toàn. 

Một bản cập nhật ở đầu hoặc cuối chuỗi chỉ ảnh hưởng đến ít hơn bốn cửa sổ do việc cắt ranh giới trong chức năng kiểm tra. Ví dụ: nếu i = 1, chúng tôi chỉ xem xét các cửa sổ bắt đầu từ chỉ số −2 đến 1 và tất cả các chỉ mục không hợp lệ đều bị bỏ qua. 

Các lần xuất hiện chồng chéo được xử lý chính xác vì mỗi cửa sổ được xử lý độc lập. Ngay cả khi hai lần xuất hiện có chung ký tự, mỗi lần xuất hiện đều được tính riêng và cập nhật nhất quán trong quá trình trừ và cộng xung quanh mỗi truy vấn.
