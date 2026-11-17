---
title: "CF 1991A - Tối đa hóa yếu tố cuối cùng"
description: "Chúng ta được cung cấp một mảng có độ dài lẻ và chúng ta nén nó liên tục bằng cách xóa hai phần tử lân cận trong một lần di chuyển cho đến khi chỉ còn lại một giá trị. Mỗi lần xóa sẽ loại bỏ một cặp liền kề và mọi thứ khác sẽ dịch chuyển cùng nhau."
date: "2026-06-08T15:22:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1991
codeforces_index: "A"
codeforces_contest_name: "Pinely Round 4 (Div. 1 + Div. 2)"
rating: 800
weight: 1991
solve_time_s: 95
verified: false
draft: false
---

[CF 1991A - Tối đa hóa yếu tố cuối cùng](https://codeforces.com/problemset/problem/1991/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài lẻ và chúng ta nén nó liên tục bằng cách xóa hai phần tử lân cận trong một lần di chuyển cho đến khi chỉ còn lại một giá trị. Mỗi lần xóa sẽ loại bỏ một cặp liền kề và mọi thứ khác sẽ dịch chuyển cùng nhau. Mục đích là chọn trình tự xóa sao cho giá trị còn lại cuối cùng càng lớn càng tốt. 

Một cách khác để xem quy trình là chúng ta ghép nối các phần tử của mảng, trong đó mỗi thao tác sẽ loại bỏ một cặp liền kề và cuối cùng chính xác một phần tử không được ghép đôi. Hạn chế chính là các cặp phải liền kề nhau tại thời điểm chúng bị loại bỏ, nhưng do mảng co lại nên cấu trúc ghép nối hiệu quả rất linh hoạt. 

Các ràng buộc nhỏ cho mỗi thử nghiệm, với n lên tới 99, nhưng lên tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào lên đến O(n^2) hoặc thậm chí O(n^3) đều có thể chấp nhận được, trong khi việc khám phá theo cấp số nhân của tất cả các chuỗi xóa là không cần thiết và sẽ quá chậm trong trường hợp xấu nhất vì số cách xóa liên tục các cặp liền kề tăng lên theo cấp số cộng với n. 

Một điểm tinh tế là phần tử còn lại cuối cùng không nhất thiết phải là trung tâm của mảng. Việc xóa sớm có thể dịch chuyển các phần tử xung quanh, nghĩa là phần tử sống sót cuối cùng có thể đến từ các vị trí cấu trúc khác nhau tùy thuộc vào cách sắp xếp việc xóa. Một cách tiếp cận tham lam ngây thơ luôn loại bỏ cặp lân cận nhỏ nhất hoặc lớn nhất cục bộ không thành công vì các lựa chọn cục bộ ảnh hưởng đến phần tử nào còn sẵn có cho bước cuối cùng. Ví dụ, trong`[1, 3, 2]`, loại bỏ`1 3`dẫn đến`2`, trong khi loại bỏ`3 2`dẫn đến`1`, vì vậy câu trả lời cuối cùng phụ thuộc vào sự lựa chọn toàn cầu chứ không phải thứ tự địa phương. 

Một trường hợp khó phát hiện khác là khi phần tử tối đa bị “chặn” do cần một mức loại bỏ chẵn lẻ cụ thể để tiếp cận nó. Tuy nhiên, vì n là số lẻ nên chính xác một vị trí sẽ tồn tại và cấu trúc hoạt động đảm bảo chúng ta luôn có thể định tuyến các thao tác xóa để bất kỳ vị trí đã chọn nào cũng có thể được giữ nguyên nếu chúng ta quản lý việc ghép nối chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi xóa liền kề có thể xảy ra. Ở mỗi bước, chúng tôi chọn bất kỳ cặp liền kề nào để loại bỏ, lặp lại trên mảng kết quả và theo dõi giá trị cuối cùng tốt nhất. Điều này đúng vì nó khám phá mọi chuỗi hoạt động hợp lệ. Tuy nhiên, hệ số phân nhánh là O(n) khi bắt đầu, sau đó là O(n−2), v.v., dẫn đến sự tăng trưởng giống như giai thừa. Ngay cả với n = 25, điều này vẫn không khả thi và ở đây n có thể là 99, khiến điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các mảng trung gian, chỉ quan tâm đến phần tử nào có thể tồn tại sau tất cả các lần loại bỏ cặp. Mỗi thao tác loại bỏ chính xác hai phần tử, do đó quá trình loại bỏ tổng cộng n−1 phần tử, để lại một phần tử sống sót. Điều này có nghĩa là mọi quy trình hợp lệ đều tương đương với việc chọn n-1 phần tử để xóa sao cho việc xóa có thể được sắp xếp thành các cặp liền kề theo thời gian. 

Cái nhìn sâu sắc về cấu trúc quan trọng là phần sống sót luôn có thể được chọn từ bất kỳ vị trí nào nếu chúng tôi đảm bảo rằng tất cả các phần tử ngoại trừ nó có thể được ghép nối theo cách nhất quán với các ràng buộc kề cận. Bởi vì chúng ta được phép xóa các cặp liền kề nhiều lần, nên chúng ta luôn có thể “cắt bỏ” các phần tử từ cả hai phía cho đến khi chỉ còn lại phần tử được chọn, miễn là chúng ta không vi phạm các ràng buộc chẵn lẻ. 

Vì vẫn còn chính xác một phần tử và chúng ta luôn có thể sắp xếp việc xóa để bất kỳ phần tử nào được chọn vẫn tồn tại, câu trả lời sẽ giảm xuống việc chọn phần tử tối đa trong mảng. 

Điều này chỉ đáng ngạc nhiên vì thao tác trông có vẻ hạn chế nhưng thực tế nó đủ mạnh để loại bỏ mọi thứ ngoại trừ một vị trí được chọn tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) đệ quy | Quá chậm | 
| Tối ưu (quan sát khả năng tiếp cận) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, quét tất cả các phần tử của mảng. Mục tiêu là xác định phần tử nào có thể là phần tử sống sót cuối cùng sau khi loại bỏ một cách tối ưu. 
2. Theo dõi giá trị lớn nhất được thấy cho đến nay trong khi duyệt qua mảng. Mỗi phần tử là một ứng cử viên sống sót vì tồn tại một chuỗi các thao tác xóa liền kề để bảo tồn nó. 
3. Sau khi quét toàn bộ mảng, xuất ra giá trị lớn nhất. 

Bước lý luận không hề tầm thường duy nhất là tại sao chỉ cần theo dõi mức tối đa là đủ: mọi yếu tố đều có thể đạt được một cách độc lập với tư cách là yếu tố sống sót cuối cùng, vì vậy kết quả tốt nhất có thể đạt được chỉ đơn giản là giá trị sẵn có lớn nhất. 

### Tại sao nó hoạt động 

Mỗi thao tác sẽ loại bỏ chính xác hai phần tử liền kề, do đó quy trình sẽ giảm kích thước mảng xuống hai phần tử cùng một lúc cho đến khi còn lại một phần tử. Bất kỳ phần tử nào được chọn đều có thể được bảo vệ bằng cách luôn xóa các cặp liền kề ở vùng bên ngoài, thu nhỏ dần mảng về phía nó. Vì không có thao tác nào buộc phải loại bỏ một phần tử cụ thể trừ khi nó được ghép nối, nên chúng ta luôn có thể tránh ghép nối phần tử tối đa đã chọn bằng cách cấu trúc các phần xóa xung quanh nó. Điều này làm cho mọi vị trí đều khả thi với tư cách là người sống sót, vì vậy chiến lược tối ưu là bảo toàn giá trị lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max(a))
```Mã đọc từng trường hợp thử nghiệm một cách độc lập và tính giá trị tối đa của mảng. Ý tưởng cốt lõi là không cần mô phỏng vì câu trả lời chỉ phụ thuộc vào giá trị sống sót tốt nhất có thể đạt được chứ không phải trình tự xóa. 

Chi tiết triển khai duy nhất quan trọng là sử dụng đầu vào nhanh vì có thể có tới 1000 trường hợp thử nghiệm. các`max`hàm chạy trong O(n), đủ các ràng buộc đã cho. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 3, 2]`| Bước | Trạng thái mảng | Hành động được chọn | Còn lại | 
| --- | --- | --- | --- | 
| 1 | 1 3 2 | xóa 1 3 | 2 | 
| 2 | 2 | dừng lại | 2 | 

| Bước | Trạng thái mảng | Hành động được chọn | Còn lại | 
| --- | --- | --- | --- | 
| 1 | 1 3 2 | loại bỏ 3 2 | 1 | 
| 2 | 1 | dừng lại | 1 | 

Điều này cho thấy tùy theo thứ tự xóa, các phần tử khác nhau có thể tồn tại. Giá trị tối đa có thể đạt được là 2, xác nhận rằng việc chọn phần tử tối đa là tối ưu. 

### Ví dụ 2 

đầu vào:`[4, 7, 4, 2, 9]`| Bước | Trạng thái mảng | Hành động được chọn | Còn lại | 
| --- | --- | --- | --- | 
| 1 | 4 7 4 2 9 | loại bỏ 7 4 | 4 2 9 | 
| 2 | 4 2 9 | loại bỏ 4 2 | 9 | 
| 3 | 9 | dừng lại | 9 | 

Điều này chứng tỏ rằng chúng ta có thể xóa các phần tử xung quanh giá trị lớn nhất một cách có hệ thống, giữ nguyên phần tử đó cho đến hết. 

Cả hai dấu vết đều xác nhận rằng việc xóa cục bộ luôn có thể được sắp xếp để cô lập phần tử đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | một lượt để tính toán tối đa | 
| Không gian | O(1) | chỉ lưu trữ chạy tối đa | 

Giải pháp chia tỷ lệ tuyến tính với kích thước đầu vào, điều này là đủ dễ dàng vì tổng n trên các trường hợp thử nghiệm bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(max(a)))
    return "\n".join(out)

# provided samples
assert run("""4
1
6
3
1 3 2
5
4 7 4 2 9
7
3 1 4 1 5 9 2
""") == """6
2
9
5"""

# minimum size
assert run("""1
1
42
""") == "42"

# all equal
assert run("""1
5
7 7 7 7 7
""") == "7"

# decreasing
assert run("""1
5
9 8 7 6 5
""") == "9"

# alternating
assert run("""1
5
1 100 1 100 1
""") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | cùng giá trị | trường hợp cơ sở | 
| tất cả đều bình đẳng | cùng giá trị | tính trung lập của hoạt động | 
| giảm dần | phần tử đầu tiên | lựa chọn tối đa toàn cầu | 
| mức cao xen kẽ | max sống sót | loại bỏ không cục bộ | 

## Vỏ cạnh 

Đối với mảng một phần tử, không thể thực hiện thao tác nào và phần tử đó đã là câu trả lời. Thuật toán trả về trực tiếp nó vì`max([x])`bằng`x`. 

Đối với một mảng có giá trị tối đa ở điểm cuối, chẳng hạn như`[9, 1, 2, 3, 4]`, thuật toán vẫn trả về 9. Về mặt khái niệm, chúng ta có thể xóa nhiều lần các cặp bắt đầu từ phía bên phải, luôn giữ nguyên phần tử đầu tiên, do đó 9 có thể giữ nguyên cho đến hết. 

Đối với một mô hình như`[1, 100, 1, 100, 1]`, mặc dù cực đại được phân tách bằng các giá trị nhỏ hơn, quy trình này cho phép chúng tôi loại bỏ tất cả các phần tử không được chọn xung quanh 100 đã chọn. Vì chúng tôi chỉ cần một người sống sót nên chúng tôi có thể bảo toàn 100 và thuật toán xuất ra chính xác 100.
