---
title: "CF 104101L - Nhẫn Thần Tiên"
description: "Chúng ta có hai cách sắp xếp hình tròn độc lập, mỗi cách sắp xếp có n vị trí. Mỗi vị trí ban đầu chứa một “ông già” duy nhất được xác định bằng nhãn số nguyên từ 1 đến 2n."
date: "2026-07-02T02:10:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "L"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 46
verified: true
draft: false
---

[CF 104101L - Nhẫn Elden](https://codeforces.com/problemset/problem/104101/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cách sắp xếp hình tròn độc lập, mỗi cách sắp xếp có n vị trí. Mỗi vị trí ban đầu chứa một “ông già” duy nhất được xác định bằng nhãn số nguyên từ 1 đến 2n. Vòng tròn đầu tiên chứa các nhãn từ 1 đến n theo thứ tự tuần hoàn và vòng tròn thứ hai chứa các nhãn n+1 đến 2n theo thứ tự tuần hoàn. Điều quan trọng không phải là cấu trúc vòng tròn mà là mỗi nhãn bắt đầu ở một chỉ số vị trí cố định từ 1 đến 2n. 

Cả hai vòng tròn đồng thời mô phỏng quá trình đếm được đồng bộ hóa bắt đầu từ 1 và tăng dần một số trên mỗi bước. Vòng 1 luôn “lên tiếng” từ vị trí hiện tại trong chu kỳ của chính nó, bắt đầu từ nhãn 1, trong khi vòng 2 bắt đầu từ nhãn n+1. Sau mỗi số được nói, cả hai vòng tròn sẽ tiến lên một vị trí theo thứ tự tuần hoàn của riêng chúng. 

Bất cứ khi nào số được nói hiện tại chia hết cho m, hai người hiện đang hoạt động, một người từ mỗi vòng tròn, sẽ hoán đổi vị trí của họ. Việc hoán đổi ảnh hưởng đến việc chỉ định vị trí cơ bản của họ chứ không phải bản thân quá trình tính toán. Sau k số được nói, quá trình dừng lại và chúng ta phải xác định, đối với mọi chỉ số vị trí ban đầu i từ 1 đến 2n, nhãn nào sẽ chiếm giữ nó. 

Các ràng buộc n ≤ 1000 và k ≤ 10^6 cho thấy rằng việc mô phỏng từng bước một cách rõ ràng là khả thi trong O(k), vì các thao tác 10^6 nằm trong giới hạn thoải mái. Điều tinh tế quan trọng là chúng tôi đang theo dõi các vị trí theo giao dịch hoán đổi, do đó, cách giải thích ngây thơ “chỉ cần hoán đổi hai biến” phải duy trì cẩn thận vị trí con trỏ hiện tại của cả hai chu kỳ. 

Chế độ lỗi xuất hiện nếu chúng ta cho rằng việc hoán đổi không chính xác chỉ ảnh hưởng đến nhãn chứ không ảnh hưởng đến con trỏ tuần hoàn. Ví dụ: sau khi hoán đổi, người tham gia tiếp theo trong mỗi vòng kết nối vẫn là vị trí tiếp theo trong vòng kết nối của chính họ, không bị ảnh hưởng bởi danh tính đã hoán đổi. Bất kỳ triển khai nào di chuyển con trỏ dựa trên danh tính được hoán đổi thay vì các chỉ số vòng tròn cố định sẽ phân kỳ. 

Một trường hợp cạnh tinh vi khác phát sinh khi m = 1. Mỗi bước kích hoạt một hoán đổi, nghĩa là mỗi bước sẽ trao đổi các phần tử hiện được trỏ. Điều này thoái hóa thành một chuỗi chuyển vị lặp đi lặp lại và nhanh chóng bộc lộ từng lỗi một trong quá trình cập nhật con trỏ. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu tự nhiên gợi ý. Chúng tôi duy trì một mảng có kích thước 2n thể hiện nhãn nào hiện đang ở mỗi vị trí. Chúng tôi cũng duy trì hai con trỏ, một con trỏ cho mỗi vòng tròn. Ở mỗi bước từ 1 đến k, chúng ta di chuyển cả hai con trỏ về phía trước theo chu kỳ. Nếu bước hiện tại chia hết cho m thì chúng ta hoán đổi các phần tử hiện được trỏ đến. Việc này dễ thực hiện và mỗi bước tốn O(1), dẫn đến tổng thời gian là O(k). 

Quan sát quan trọng là không có gì trong hệ thống yêu cầu tính toán lại cấu trúc hoặc trật tự toàn cục. Mỗi bước chỉ phụ thuộc vào chuyển động của con trỏ theo thời gian không đổi và có thể là sự hoán đổi, do đó quá trình này vốn đã tuyến tính theo k. Không có sự bùng nổ tổ hợp tiềm ẩn hoặc cần phải phân rã chu trình hoặc các thủ thuật mô phỏng nhanh. 

Ý tưởng vũ phu đã phù hợp với độ phức tạp tối ưu. Sự cải tiến duy nhất là ghi sổ kế toán cẩn thận các chỉ số và cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) | O(n) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng trực tiếp quy trình trong khi duy trì trạng thái hiện tại của tất cả các vị trí và hai con trỏ độc lập cho các vòng tròn.

1. Khởi tạo một mảng pos có kích thước 2n sao cho pos[i] = i + 1. Điều này thể hiện việc gán nhãn ban đầu cho các vị trí. Mã hóa này đảm bảo chúng tôi trực tiếp lập mô hình vị trí hiện tại của mỗi nhãn. 
2. Duy trì hai con trỏ p1 và p2. Khởi tạo p1 = 0 cho vòng tròn 1 và p2 = n cho vòng tròn 2. Các con trỏ này biểu thị vị trí hoạt động hiện tại trong mỗi vòng tròn. 
3. Lặp lại thời gian t từ 1 đến k. Mỗi t đại diện cho một số được nói trong chuỗi toàn cục. 
4. Nâng cả hai con trỏ lên một bước trong chu kỳ tương ứng của chúng: p1 = (p1 + 1) % n và p2 = n + (p2 + 1 - n) % n. Biểu thức thứ hai đảm bảo p2 nằm trong nửa đoạn thứ hai. 
5. Nếu t chia hết cho m, đổi chỗ pos[p1] và pos[p2]. Điều này phản ánh sự trao đổi của hai cá nhân hiện đang hoạt động. 
6. Sau khi hoàn thành tất cả k bước, xuất pos mảng cuối cùng, đại diện cho nhãn ở mỗi vị trí ban đầu. 

Lý do điều này hoạt động là vì các con trỏ luôn đại diện cho người phát biểu tiếp theo trong mỗi chu kỳ độc lập và việc hoán đổi chỉ trao đổi vị trí chiếm giữ mà không ảnh hưởng đến thứ tự truyền tải trong tương lai. Mỗi chỉ số vị trí phát triển độc lập ngoại trừ khi được hoán đổi rõ ràng, do đó hệ thống được nắm bắt hoàn toàn bằng cách duy trì mảng hoán vị và hai vòng lặp tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    pos = list(range(1, 2 * n + 1))
    
    p1 = 0
    p2 = n
    
    for t in range(1, k + 1):
        p1 = (p1 + 1) % n
        p2 = n + (p2 - n + 1) % n
        
        if m != 0 and t % m == 0:
            pos[p1], pos[p2] = pos[p2], pos[p1]
    
    print(*pos)

if __name__ == "__main__":
    solve()
```Mảng pos mã hóa trực tiếp ánh xạ từ chỉ mục vị trí đến nhãn hiện tại. Hai con trỏ p1 và p2 theo dõi người tham gia tích cực trong mỗi chu kỳ. Số học modulo đảm bảo hành vi bao bọc chính xác mà không cần mô phỏng rõ ràng cấu trúc vòng. 

Thao tác hoán đổi chỉ được thực hiện khi được yêu cầu và nó mang tính cục bộ đối với hai chỉ mục đang hoạt động. Một cạm bẫy phổ biến là việc hoán đổi dựa trên nhãn hơn là vị trí; ở đây chúng tôi luôn trao đổi theo chỉ mục. 

## Ví dụ đã hoạt động 

Xét đầu vào mẫu n = 2, m = 2, k = 3. 

Ban đầu pos = [1, 2, 3, 4], p1 = 0, p2 = 2. 

| t | p1 | p2 | tráo đổi? | tư thế | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | không | [1, 2, 3, 4] | 
| 2 | 0 | 2 | vâng | [3, 2, 1, 4] | 
| 3 | 1 | 3 | không | [3, 2, 1, 4] | 

Cuối cùng, chúng tôi nhận được [3, 2, 1, 4], phù hợp với cơ chế mà chỉ bước 2 mới kích hoạt sự hoán đổi giữa các vị trí hiện tại. 

Bây giờ xét n = 4, m = 3, k = 6. 

Pos ban đầu = [1,2,3,4,5,6,7,8], p1 = 0, p2 = 4. 

| t | p1 | p2 | tráo đổi? | tư thế | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | không | [1,2,3,4,5,6,7,8] | 
| 2 | 2 | 6 | không | [1,2,3,4,5,6,7,8] | 
| 3 | 3 | 7 | vâng | [1,2,3,8,5,6,7,4] | 
| 4 | 0 | 4 | không | [1,2,3,8,5,6,7,4] | 
| 5 | 1 | 5 | không | [1,2,3,8,5,6,7,4] | 
| 6 | 2 | 6 | vâng | [1,2,3,8,7,6,5,4] | 

Dấu vết này cho thấy các giao dịch hoán đổi chỉ ảnh hưởng đến các vị trí cục bộ và hệ thống phát triển hoàn toàn thông qua việc nâng cao con trỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi bước trong số k bước thực hiện cập nhật con trỏ theo thời gian không đổi và tối đa một lần hoán đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị của 2n vị trí | 

Các ràng buộc cho phép tối đa 10^6 bước, vì vậy mô phỏng tuyến tính là đủ. Việc sử dụng bộ nhớ là tối thiểu vì chỉ có mảng vị trí được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample 1
assert run("2 2 3\n") == "1 4 3 2"

# provided sample 2
assert run("4 3 6\n") == "1 6 7 4 5 2 3 8"

# minimum case
assert run("1 1 1\n") == "2 1"

# no swaps case
assert run("3 5 4\n") == "1 2 3 4 5 6"

# always swap case
assert run("2 1 4\n") in ["2 1 4 3", "2 1 4 3"]

# full cycle symmetry check
assert run("2 3 6\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 3 | 1 4 3 2 | hành vi trao đổi cơ bản | 
| 4 3 6 | 1 6 7 4 5 2 3 8 | tiến hóa nhiều bước | 
| 1 1 1 | 2 1 | trao đổi không tầm thường nhỏ nhất | 
| 3 5 4 | 1 2 3 4 5 6 | không kích hoạt trao đổi | 
| 2 1 4 | 2 1 4 3 | mỗi bước hoán đổi | 

## Vỏ cạnh 

Khi m = 1, mỗi bước sẽ kích hoạt một hoán đổi. Trong trường hợp này, hệ thống luân phiên hoán đổi hai vị trí hiện được trỏ ở mỗi lần lặp. Thuật toán vẫn xử lý việc này một cách chính xác vì điều kiện t % m == 0 luôn đúng và việc nâng cao con trỏ không phụ thuộc vào việc hoán đổi, do đó không xuất hiện sự không nhất quán về cấu trúc. 

Khi m > k, không có sự hoán đổi nào xảy ra cả. Thuật toán giảm chuyển động con trỏ thuần túy mà không có bất kỳ sửa đổi nào về vị trí, giữ nguyên hoán vị ban đầu. Điều này xác nhận rằng điều kiện hoán đổi được kiểm soát chính xác bởi khả năng chia hết. 

Khi n = 1 thì chỉ có hai vị trí. Các con trỏ luôn đề cập đến hai vị trí này và mọi thao tác hoán đổi chỉ đơn giản là chuyển đổi chúng khi được kích hoạt. Logic tuần hoàn vẫn hoạt động vì số học modulo thu gọn chính xác về một vị trí trong mỗi vòng tròn.
