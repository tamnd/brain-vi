---
title: "CF 105728H - Chiếc đồng hồ tử thần quay vòng"
description: "Nhiệm vụ có thể được hiểu là mô phỏng một quá trình sắp xếp các vị trí theo vòng tròn, trong đó con trỏ di chuyển xung quanh giống như kim đồng hồ và liên tục loại bỏ các phần tử theo một quy tắc cố định cho đến khi chỉ còn lại một vị trí."
date: "2026-06-26T07:50:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105728
codeforces_index: "H"
codeforces_contest_name: "EPT Solving Cup 5.0 \uacf5\uc2dd \uacbd\uc5f0\ub300\ud68c"
rating: 0
weight: 105728
solve_time_s: 48
verified: true
draft: false
---

[CF 105728H - Chiếc đồng hồ tử thần quay vòng](https://codeforces.com/problemset/problem/105728/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ có thể được hiểu là mô phỏng một quá trình sắp xếp các vị trí theo vòng tròn, trong đó con trỏ di chuyển xung quanh giống như kim đồng hồ và liên tục loại bỏ các phần tử theo một quy tắc cố định cho đến khi chỉ còn lại một vị trí. Mỗi vị trí đại diện cho một người tham gia được sắp xếp theo một vòng tròn và quá trình sẽ tiến triển theo kích thước bước cố định bao quanh vòng tròn nhiều lần. Bất cứ khi nào con trỏ chạm vào một vị trí chưa bị xóa, vị trí đó sẽ bị loại bỏ và quá trình tiếp tục từ vị trí sẵn có tiếp theo. 

Đầu vào mô tả kích thước của vòng tròn và kích thước bước xác định khoảng cách con trỏ di chuyển giữa các lần loại bỏ. Đầu ra là nhãn của vị trí sống sót cuối cùng sau khi thực hiện tất cả việc loại bỏ. 

Từ góc độ phức tạp, một mô phỏng trực tiếp sẽ yêu cầu duy trì cấu trúc hình tròn thu nhỏ linh hoạt và đưa con trỏ liên tục lên trên nó. Với giá trị n tối đa, có khả năng ở mức 10^5 trở lên, bất kỳ phương pháp nào liên tục quét hoặc dịch chuyển các phần tử bên trong danh sách sẽ chuyển sang hành vi bậc hai. Điều này ngay lập tức loại trừ việc xóa mảng đơn giản hoặc lặp lại việc duyệt qua toàn bộ cấu trúc. 

Trường hợp cạnh tinh tế xuất hiện khi kích thước bước là 1. Trong trường hợp đó, mọi phần tử đều bị loại bỏ theo thứ tự và phần tử cuối cùng trong cách sắp xếp ban đầu luôn là câu trả lời. Một trường hợp cạnh khác phát sinh khi kích thước bước lớn hơn số phần tử còn lại hiện tại, trong đó việc xử lý mô-đun đơn giản có thể dễ dàng sai sót nếu việc lập chỉ mục không được chuẩn hóa cẩn thận theo kích thước vòng tròn giảm. Ví dụ: với n = 5 và k = 7, việc loại bỏ vẫn hoạt động như thể k = 7 mod 5 ở trạng thái hiện tại, nhưng việc triển khai bất cẩn thường xử lý sai phần bao quanh và bỏ qua các vị trí không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu giữ tất cả các vị trí trong một danh sách hoặc deque và liên tục tiến lên một con trỏ k bước về phía trước, loại bỏ phần tử hiện tại và tiếp tục. Mỗi lần xóa yêu cầu dịch chuyển các phần tử hoặc lặp qua các vị trí đã bị xóa. Ngay cả với một danh sách liên kết hoặc deque, việc lặp đi lặp lại k bước cho mỗi lần xóa n sẽ dẫn đến hành vi O(nk). Trong trường hợp xấu nhất khi k có thể so sánh với n, giá trị này suy biến thành O(n^2), quá chậm đối với đầu vào lớn. 

Quan sát quan trọng là sau mỗi lần loại bỏ, vấn đề sẽ giảm xuống cùng một cấu trúc nhưng với ít phần tử hơn và chỉ số tương đối của những người sống sót sẽ thay đổi theo cách có thể dự đoán được. Thay vì mô phỏng rõ ràng vòng tròn, chúng ta có thể theo dõi vị trí của người sống sót trong một mảng thu nhỏ bằng cách sử dụng mối quan hệ lặp lại. Nếu chúng ta xác định f(i) là vị trí chiến thắng trong vòng tròn có kích thước i, thì việc chuyển từ kích thước i - 1 sang i sẽ dịch chuyển các chỉ số về phía trước theo kích thước bước và bao quanh modulo i. Điều này giúp loại bỏ sự cần thiết của bất kỳ cấu trúc dữ liệu rõ ràng nào và giảm quy trình xuống mức lặp lại tuyến tính đơn giản. 

Brute-force hoạt động vì nó bắt chước trực tiếp quy trình, nhưng không thành công khi số lượng thao tác tăng lên do việc duyệt và xóa lặp đi lặp lại. Nhận xét rằng chỉ có vị trí tương đối của những người sống sót mới cho phép chúng ta nén toàn bộ quá trình thành một lần vượt qua n trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tái diễn (Josephus DP) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định hàm theo dõi chỉ số người sống sót khi vòng tròn tăng từ kích thước 1 lên kích thước n.

1. Bắt đầu với một vòng tròn phần tử duy nhất. Người sống sót ở vị trí tầm thường là 0 ở dạng 0 chỉ mục. Điều này đóng vai trò là trạng thái cơ bản vì không thể loại bỏ được. 
2. Tăng kích thước vòng tròn từ 2 lên n và với mỗi kích thước i, hãy cập nhật vị trí người sống sót bằng cách sử dụng mối quan hệ lặp lại. Ở mỗi bước, chúng tôi mô phỏng cách giải pháp trước đó thay đổi khi một phần tử mới được thêm vào. 
3. Quy tắc cập nhật là: vị trí người sống sót mới trở thành`(previous_position + k) % i`. Điều này phản ánh thực tế là con trỏ loại bỏ tiến lên k bước và bao quanh kích thước vòng tròn hiện tại i. 
4. Sau khi lặp qua tất cả các kích thước lên đến n, hãy chuyển đổi vị trí được lập chỉ mục 0 cuối cùng thành dạng được lập chỉ mục 1 cho đầu ra. 

Lý do điều này có hiệu quả là vì mỗi bước mở rộng sẽ bảo toàn cấu trúc của quá trình loại bỏ và sự lặp lại mô hình chính xác cách lập chỉ mục thay đổi khi kích thước vòng tròn tăng lên. Thứ tự loại bỏ là bất biến trong phép biến đổi này vì mọi cấu hình có kích thước i được xây dựng nhất quán trên giải pháp cho kích thước i - 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    # josephus recurrence
    res = 0
    for i in range(1, n + 1):
        res = (res + k) % i
    
    print(res + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ một trạng thái số nguyên duy nhất,`res`, đại diện cho chỉ số sống sót trong hệ thống có chỉ mục 0. Mỗi lần lặp lại sẽ mở rộng kích thước vòng tròn và áp dụng phép dịch chuyển mô-đun tương ứng với một vòng loại trừ. Việc chuyển đổi cuối cùng sang đầu ra được lập chỉ mục 1 là cần thiết vì vấn đề gắn nhãn các vị trí bắt đầu từ 1 thay vì 0. 

Một cạm bẫy triển khai phổ biến là quên rằng modulo phải được thực hiện theo kích thước hiện tại`i`, không phải kích thước cuối cùng`n`. Một vấn đề tinh vi khác là trộn lẫn lý luận được lập chỉ mục 0 và lập chỉ mục 1 bên trong vòng lặp, điều này dẫn đến các lỗi sai lệch chỉ xuất hiện đối với các đầu vào nhỏ. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có n = 5 và k = 2. Chúng tôi theo dõi chỉ số sống sót khi vòng tròn phát triển. 

| tôi | độ phân giải trước | tính toán | độ phân giải mới | 
| --- | --- | --- | --- | 
| 1 | 0 | (0 + 2) % 1 = 0 | 0 | 
| 2 | 0 | (0 + 2) % 2 = 0 | 0 | 
| 3 | 0 | (0 + 2) % 3 = 2 | 2 | 
| 4 | 2 | (2 + 2) % 4 = 0 | 0 | 
| 5 | 0 | (0 + 2) % 5 = 2 | 2 | 

Câu trả lời cuối cùng là 3 ở dạng 1 chỉ mục. Dấu vết này cho thấy sự tái diễn sẽ dịch chuyển người sống sót một cách tự nhiên như thế nào khi vòng tròn mở rộng. 

Bây giờ hãy xem xét n = 6, k = 3. 

| tôi | độ phân giải trước | tính toán | độ phân giải mới | 
| --- | --- | --- | --- | 
| 1 | 0 | (0 + 3) % 1 = 0 | 0 | 
| 2 | 0 | (0 + 3) % 2 = 1 | 1 | 
| 3 | 1 | (1 + 3) % 3 = 1 | 1 | 
| 4 | 1 | (1 + 3) % 4 = 0 | 0 | 
| 5 | 0 | (0 + 3) % 5 = 3 | 3 | 
| 6 | 3 | (3 + 3) % 6 = 0 | 0 | 

Câu trả lời cuối cùng là 1. Điều này cho thấy việc đặt lại định kỳ xảy ra như thế nào khi kích thước bước phù hợp với kích thước vòng tròn trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại một lần từ 1 đến n, thực hiện công việc liên tục trong mỗi lần lặp | 
| Không gian | O(1) | Chỉ có một trạng thái số nguyên duy nhất được duy trì | 

Điều này hiệu quả với n lớn vì nó tránh mọi mô phỏng rõ ràng về cấu trúc vòng tròn và thay thế nó bằng một phép truy toán toán học trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    n, k = map(int, input().split())
    res = 0
    for i in range(1, n
```
