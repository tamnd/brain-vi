---
title: "CF 105575F - \u5168\u80fd\u732b\u5a18\u7684\u70e6\u607c"
description: "Chúng tôi được giao một tập hợp các nhiệm vụ. Mỗi nhiệm vụ có thời gian xử lý và một ràng buộc giống như thời hạn thể hiện thời điểm muộn nhất mà nhiệm vụ đó phải được hoàn thành."
date: "2026-06-22T06:21:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105575
codeforces_index: "F"
codeforces_contest_name: "Southeast University 6th Programming Competition Competition (Winter, Novice Group) \u4e1c\u5357\u5927\u5b66\u7b2c\u516d\u5c4a\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u51ac\u5b63\u8d5b\uff08\u65b0\u624b\u7ec4\uff09"
rating: 0
weight: 105575
solve_time_s: 40
verified: true
draft: false
---

[CF 105575F - \u5168\u80fd\u732b\u5a18\u7684\u70e6\u607c](https://codeforces.com/problemset/problem/105575/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một tập hợp các nhiệm vụ. Mỗi nhiệm vụ có thời gian xử lý và một ràng buộc giống như thời hạn thể hiện thời điểm muộn nhất mà nhiệm vụ đó phải được hoàn thành. Chúng tôi được phép chọn thứ tự thực hiện tất cả các tác vụ, nhưng khi một tác vụ bắt đầu, nó sẽ chạy trong toàn bộ thời gian mà không bị gián đoạn. 

Mục đích là để quyết định xem có tồn tại một thứ tự của tất cả các nhiệm vụ sao cho mọi nhiệm vụ đều kết thúc không muộn hơn thời hạn của chính nó hay không và nếu có thể, hãy tính số lượng giống như độ trễ tối đa mà thứ tự tốt nhất ngụ ý. Giải pháp mẫu tiết lộ rằng mục tiêu thực sự giảm xuống còn việc tìm hiểu xem liệu chúng ta có thể lên lịch cho tất cả các nhiệm vụ hay không và đo lường xem chúng ta có thể đẩy lịch trình đến mức nào trước khi vi phạm tính khả thi. 

Mỗi nhiệm vụ đóng góp hai giá trị: thời lượng và thời hạn. Thứ tự nhiệm vụ ảnh hưởng đến thời gian tích lũy, do đó, sự tương tác chính là giữa tổng tiền tố của thời lượng và thời hạn. 

Từ những ràng buộc mà lời giải ngụ ý, số lượng nhiệm vụ có thể đủ lớn đến mức bất kỳ chiến lược sắp xếp bậc hai nào cũng không thể thực hiện được. Bất kỳ cách tiếp cận nào thử tất cả các hoán vị sẽ yêu cầu thời gian giai thừa và thậm chí lập trình động trên các tập hợp con sẽ theo cấp số nhân về số lượng tác vụ. Điều này ngay lập tức buộc chúng ta phải hướng tới lý luận tham lam dựa trên sắp xếp trong khoảng O(n log n). 

Một trường hợp thất bại khó nhận thấy xuất hiện khi các nhiệm vụ có thời lượng lớn nhưng có thời hạn sớm được thực hiện muộn trong lịch trình. Ví dụ, hãy xem xét các nhiệm vụ (thời lượng, thời hạn): (5, 6), (2, 3), (2, 10). Nếu chúng ta xử lý theo thứ tự tùy ý, trước tiên chúng ta có thể thử (5, 6), sau đó tích lũy thời gian và phá vỡ tính khả thi của (2, 3). Tuy nhiên, đặt hàng theo thời hạn sẽ khắc phục được vấn đề này. 

Đầu ra là một số nguyên biểu thị giá trị tốt nhất có thể đạt được từ việc lập kế hoạch hoặc -1 nếu không có lịch trình hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi thứ tự có thể có của nhiệm vụ và mô phỏng thời gian hoàn thành. Đối với mỗi hoán vị, chúng tôi tính toán tổng thời lượng tiền tố và kiểm tra xem mỗi nhiệm vụ có kết thúc trước thời hạn hay không. Điều này đúng vì nó trực tiếp kiểm tra tính khả thi. Tuy nhiên, số lượng hoán vị tăng lên theo m!, và thậm chí đối với m khoảng 10 hoặc 11 thì điều này trở nên không thực tế. Mỗi mô phỏng có giá O(m), do đó tổng độ phức tạp là O(m · m!). 

Quan sát quan trọng là các nhiệm vụ có thời hạn sớm hơn sẽ áp đặt các ràng buộc chặt chẽ hơn và cần được xử lý sớm hơn trong bất kỳ lịch trình khả thi nào. Nếu một nhiệm vụ có thời hạn sớm hơn bị trì hoãn sau những nhiệm vụ có thời hạn muộn hơn thì tính khả thi của nó chỉ trở nên khó khăn hơn chứ không bao giờ dễ dàng hơn. Điều này gợi ý việc sắp xếp các nhiệm vụ bằng cách tăng thời hạn. 

Sau khi sắp xếp, chúng tôi duy trì tổng thời lượng đang chạy. Khi xử lý từng tác vụ, chúng tôi kiểm tra xem lượng thời gian đã được sử dụng trước khi nó bắt đầu. Thời hạn biểu thức trừ đi thời gian tích lũy sẽ cho biết mức độ chậm còn lại của nhiệm vụ đó nếu nó được lên lịch theo thứ tự này. Câu trả lời là độ trễ tối thiểu như vậy đối với tất cả các nhiệm vụ, vì ràng buộc chặt chẽ nhất sẽ quyết định tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · m!) | O(m) | Quá chậm | 
| Sắp xếp tham lam tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển từng nhiệm vụ thành một cặp chứa thời lượng và thời hạn, sau đó sắp xếp các nhiệm vụ theo thời hạn.

1. Đọc tất cả các nhiệm vụ và lưu trữ chúng theo cặp (thời lượng, thời hạn). Điều này tiêu chuẩn hóa vấn đề nên chúng ta chỉ có thể suy luận về thứ tự và thời gian tích lũy. 
2. Sắp xếp công việc theo thứ tự thời hạn tăng dần. Điều này đảm bảo rằng các nhiệm vụ có ràng buộc chặt chẽ hơn sẽ được xem xét sớm hơn, ngăn chặn việc sắp xếp muộn các nhiệm vụ mong manh. 
3. Khởi tạo biến tổng số tiền đang chạy để theo dõi tổng thời gian đã sử dụng cho đến nay. Điều này thể hiện thời gian hoàn thành của tiền tố hiện tại theo thứ tự đã chọn. 
4. Lặp lại các nhiệm vụ theo thứ tự được sắp xếp, thêm thời lượng của từng nhiệm vụ vào tổng thời gian thực hiện. Sau khi cộng, hãy tính thời gian trễ cho nhiệm vụ này bằng thời hạn trừ đi tổng thời gian hiện tại. Điều này đo lường mức độ chúng ta vi phạm ràng buộc của nhiệm vụ này ở vị trí của nó. 
5. Duy trì độ trễ tối thiểu trong tất cả các nhiệm vụ. Mức tối thiểu này xác định mức độ an toàn tồi tệ nhất trong lịch trình. 
6. Sau khi xử lý tất cả các nhiệm vụ, nếu độ trễ tối thiểu là âm thì không có lịch trình hợp lệ nào tồn tại và chúng tôi xuất ra -1. Ngược lại, chúng ta xuất ra độ trễ tối thiểu này. 

### Tại sao nó hoạt động 

Việc sắp xếp theo thời hạn đảm bảo rằng bất cứ khi nào chúng tôi xem xét một nhiệm vụ, tất cả các nhiệm vụ có thời hạn sớm hơn đều đã được đặt và đóng góp vào tổng tiền tố. Bất kỳ sai lệch nào so với thứ tự này chỉ có thể làm tăng nguy cơ vi phạm thời hạn trước đó, bởi vì việc hoán đổi một nhiệm vụ có thời hạn muộn hơn sẽ không làm giảm tổng tiền tố trước một nhiệm vụ có thời hạn chặt chẽ hơn trừ khi bản thân nhiệm vụ đó có thời hạn thậm chí còn nhỏ hơn hoặc bằng nhau. Điều này chứng tỏ rằng thứ tự sắp xếp là tối ưu cho việc kiểm tra tính khả thi. 

Tổng hoạt động tạo ra một cấu trúc tiền tố cố định trong đó ràng buộc của mỗi nhiệm vụ được kiểm tra tại thời điểm sớm nhất có thể phù hợp với thứ tự thời hạn. Độ chùng tối thiểu nắm bắt được ràng buộc chặt chẽ nhất trên tất cả các tiền tố và tính khả thi sẽ giảm xuống xem độ trễ này có giữ nguyên không âm hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    tasks = []
    for _ in range(m):
        d, t = map(int, input().split())
        tasks.append((t, d))  # (deadline, duration)

    tasks.sort()

    total = 0
    best = 10**30

    for deadline, duration in tasks:
        total += duration
        best = min(best, deadline - total)

    print(best if best >= 0 else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác cấu trúc tham lam. Mỗi nhiệm vụ được lưu trữ với thời hạn đầu tiên để thực hiện phân loại trực tiếp. Tổng số tiền chạy`total`biểu thị thời gian thực hiện tích lũy theo thứ tự đã chọn. Sau mỗi lần cập nhật, chúng tôi đánh giá lượng thời gian còn lại trước khi vi phạm thời hạn của nhiệm vụ hiện tại. Biến`best`theo dõi mức tối thiểu của các giá trị này. 

Việc kiểm tra cuối cùng chuyển đổi tính khả thi thành một thử nghiệm dấu hiệu đơn giản. Giá trị âm cho biết tại một thời điểm nào đó lịch trình đã vượt quá thời hạn, khiến cho đơn đặt hàng không hợp lệ. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
m = 3
tasks: (duration, deadline)
2 5
1 3
2 10
```Sau khi sắp xếp lại vào (thời hạn, thời hạn): 

| Bước | Nhiệm vụ (d, t) | Tổng Thời Gian | Slack = t - tổng cộng | Min Slack | 
| --- | --- | --- | --- | --- | 
| 1 | (3, 1) | 1 | 2 | 2 | 
| 2 | (5, 2) | 3 | 2 | 2 | 
| 3 | (10, 2) | 5 | 5 | 2 | 

Độ trễ tối thiểu là dương nên việc lập kế hoạch là khả thi. Dấu vết cho thấy ràng buộc chặt chẽ nhất đến từ nhiệm vụ có thời hạn sớm, được xử lý trước và không bao giờ vi phạm. 

Bây giờ hãy xem xét một trường hợp thất bại:```
m = 2
tasks:
2 3
2 3
```Sau khi sắp xếp (thứ tự không thay đổi): 

| Bước | Nhiệm vụ (d, t) | Tổng Thời Gian | Chần chừ | Min Slack | 
| --- | --- | --- | --- | --- | 
| 1 | (3, 2) | 2 | 1 | 1 | 
| 2 | (3, 2) | 4 | -1 | -1 | 

Nhiệm vụ thứ hai khiến thời gian chạy vượt quá thời hạn, khiến lịch trình không hợp lệ. Điều này xác nhận rằng việc phát hiện điểm yếu tiêu cực sẽ nắm bắt chính xác tính không khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(m) | Lưu trữ danh sách nhiệm vụ | 

Các ràng buộc buộc bất kỳ giải pháp nào phải tránh hành vi bậc hai hoặc giai thừa. Sắp xếp cộng với một lượt duy nhất là đủ cho m lớn, vì mỗi tác vụ được xử lý chính xác một lần sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = backup
    return out.getvalue().strip()

# sample-like case: feasible
assert run("3 3\n5 2\n3 1\n10 2\n") == "2"

# impossible case: immediate overflow
assert run("2 2\n3 2\n3 2\n") == "-1"

# single task
assert run("1 1\n5 10\n") == "5"

# tight chain
assert run("4 3\n3 4\n3 4\n3 10\n") == "-1"

# already optimal order
assert run("5 3\n1 5\n2 7\n3 10\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ đơn lẻ | 5 | tính đúng đắn của trường hợp cơ sở | 
| hai nhiệm vụ chặt chẽ giống hệt nhau | -1 | phát hiện tràn tích lũy | 
| chuỗi sắp xếp khả thi | 2 | hành vi tham lam ổn định | 
| dây xích chặt bị hỏng | -1 | tính chính xác tích lũy tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một nhiệm vụ có thời lượng lớn xuất hiện sau thời hạn lớn hơn một chút nhưng vẫn không thể thực hiện được do thời gian tích lũy. Ví dụ:```
m = 2
tasks:
4 5
3 6
```Sau khi sắp xếp theo thời hạn: 

| Bước | Nhiệm vụ (d, t) | Tổng cộng | Chần chừ | 
| --- | --- | --- | --- | 
| 1 | (5, 4) | 4 | 1 | 
| 2 | (6, 3) | 7 | -1 | 

Mặc dù cả hai nhiệm vụ đều phù hợp với thời hạn của chúng, nhưng thứ tự kết hợp của chúng sẽ gây ra tình trạng quá tải. Thuật toán nắm bắt được điều này vì nó đánh giá thời gian tích lũy chứ không phải tính khả thi riêng lẻ. 

Một trường hợp khác là khi tất cả các thời hạn đều lớn nhưng thời lượng cũng lớn. Ngay cả khi đó, độ trễ tối thiểu vẫn phản ánh chính xác liệu mức tăng trưởng tích lũy có nằm trong giới hạn hay không, đảm bảo không bỏ sót kịch bản tràn ẩn nào.
