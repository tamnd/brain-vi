---
title: "CF 104261C - Biến chứng hiệu chuẩn"
description: "Chúng ta có hai mảng có cùng độ dài và chúng ta muốn biến đổi chúng sao cho mọi phần tử trên cả hai mảng đều bằng một giá trị chung duy nhất."
date: "2026-07-01T21:40:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104261
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 2 (Beginner)"
rating: 0
weight: 104261
solve_time_s: 58
verified: true
draft: false
---

[CF 104261C - Các biến chứng khi hiệu chỉnh](https://codeforces.com/problemset/problem/104261/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có cùng độ dài và chúng ta muốn biến đổi chúng sao cho mọi phần tử trên cả hai mảng đều bằng một giá trị chung duy nhất. Các bước di chuyển duy nhất được phép là tăng một phần tử trong mảng thứ nhất lên 1 hoặc giảm một phần tử trong mảng thứ hai xuống 1. Mỗi thay đổi như vậy tốn một thao tác và chúng tôi muốn giảm thiểu tổng số thao tác cần thiết để làm cho tất cả các số trong cả hai mảng giống hệt nhau. Nếu không thể đạt được giá trị chung bằng cách sử dụng những bước đi này, chúng tôi phải báo cáo là không thể thực hiện được. 

Quan sát quan trọng là các phần tử trong mảng thứ nhất chỉ có thể di chuyển lên trên và các phần tử trong mảng thứ hai chỉ có thể di chuyển xuống dưới. Điều đó ngay lập tức hạn chế giá trị mục tiêu cuối cùng có thể là bao nhiêu. Giá trị cuối cùng ít nhất phải lớn bằng tất cả các phần tử trong mảng đầu tiên và tối đa bằng tất cả các phần tử trong mảng thứ hai. Nếu hai ràng buộc này không trùng nhau thì sẽ không có mục tiêu khả thi. 

Các ràng buộc cho phép tối đa 100000 phần tử có giá trị lên tới 1e9, do đó, mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Việc sắp xếp là không cần thiết nhưng cần phải quét điểm cực trị toàn cục. Cách tiếp cận O(n^2) thử mọi mục tiêu có thể có đối với tất cả các phần tử sẽ quá chậm vì nó sẽ yêu cầu tới 1e10 thao tác trong trường hợp xấu nhất. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu giá trị tối đa của mảng đầu tiên lớn hơn giá trị tối thiểu của mảng thứ hai thì không có giải pháp nào tồn tại ngay cả khi hầu hết các phần tử có vẻ tương thích. Ví dụ: a = [10], b = [1] thất bại ngay lập tức vì mảng thứ nhất không thể giảm và mảng thứ hai không thể tăng. Một trường hợp tinh tế khác là khi các mảng đã thỏa mãn sự chồng chéo nhưng chi phí tối ưu chỉ phụ thuộc vào các phần tử biên chứ không phụ thuộc vào việc ghép từng phần tử. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi giá trị cuối cùng T có thể có giữa giá trị tối thiểu và tối đa xuất hiện trong một trong hai mảng. Đối với mỗi ứng cử viên T, chúng tôi tính toán chi phí: mọi phần tử trong mảng đầu tiên đóng góp max(0, T - a[i]) vì nó phải được tăng lên T và mọi phần tử trong mảng thứ hai đóng góp max(0, b[i] - T) vì nó phải giảm xuống T. Chúng tôi lấy mức tối thiểu trên tất cả T. 

Điều này đúng vì mọi giải pháp hợp lệ đều phải hội tụ đến một số nguyên T và công thức chi phí đo lường chính xác các hoạt động cần thiết cho T đó. Tuy nhiên, phạm vi giá trị T có thể kéo dài đến 1e9 và việc đánh giá từng giá trị là không thể. Ngay cả khi chúng tôi giới hạn các ứng cử viên ở các giá trị duy nhất trong mảng, chúng tôi vẫn có các ứng cử viên O(n) và mỗi đánh giá là O(n), cho ra O(n^2), quá chậm đối với 1e5 phần tử. 

Cái nhìn sâu sắc quan trọng là hàm chi phí lồi trên dòng số nguyên. Khi T tăng, chi phí do mảng đầu tiên đóng góp tăng tuyến tính, trong khi chi phí của mảng thứ hai giảm tuyến tính. Tổng chi phí là tuyến tính từng phần với một mức tối thiểu duy nhất. Điều đó có nghĩa là chúng ta không cần tìm kiếm toàn bộ miền; mức tối thiểu xảy ra khi “áp lực” từ việc tăng mảng đầu tiên cân bằng “áp lực” từ việc giảm mảng thứ hai. Trên thực tế, chúng ta chỉ cần đảm bảo tính khả thi và sau đó đánh giá chi phí ở ranh giới nhằm giảm thiểu chuyển động, hóa ra là bất kỳ T hợp lệ nào trong giao lộ và lựa chọn tối ưu được xác định bằng cách thêm tiền tố vào mọi thứ cho mục tiêu nhất quán trong giới hạn. 

Điều này làm giảm vấn đề trong việc tìm kiếm xem có tồn tại sự chồng chéo hay không và sau đó tính toán chi phí để căn chỉnh mọi thứ cho phù hợp với bất kỳ mục tiêu hợp lệ nào, điển hình là ranh giới giảm thiểu tổng độ lệch tuyệt đối theo các ràng buộc về hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả T | O(n * R) hoặc O(n^2) | O(1) | Quá chậm | 
| Quét tuyến tính tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta trình bày lại vấn đề bằng cách chọn một số nguyên T sao cho tất cả các giá trị trong mảng đầu tiên được nâng lên T và tất cả các giá trị trong mảng thứ hai được hạ xuống T, chỉ sử dụng các phép toán được phép. 

1. Tính giá trị lớn nhất ở mảng thứ nhất và giá trị nhỏ nhất ở mảng thứ hai. Những điều này xác định giới hạn khả thi cho T. Nếu mức tối đa của mảng đầu tiên vượt quá mức tối thiểu của mảng thứ hai thì không tồn tại T chung nào tôn trọng cả hai ràng buộc chuyển động, do đó câu trả lời ngay lập tức là không thể. 
2. Nếu khả thi, chúng ta thực sự không cần liệt kê T. Bất kỳ T hợp lệ nào trong khoảng [max(a), min(b)] đều có thể truy cập được và mang lại một giải pháp hợp lệ, nhưng chúng ta phải tính số phép toán tối thiểu trong số tất cả các lựa chọn như vậy. 
3. Quan sát rằng đối với bất kỳ T cố định nào, chi phí sẽ được phân chia độc lập giữa các mảng. Mọi a[i] đều đóng góp T - a[i], bởi vì tất cả đều ∼ T trong một nghiệm hợp lệ. Mọi b[i] đóng góp b[i] - T, vì tất cả đều ≥ T. 
4. Tổng các đóng góp này sẽ cho một biểu thức tuyến tính trong T. Phần đầu tiên đóng góp n_T - sum(a), và phần thứ hai đóng góp tổng (b) - n_T, do đó T hủy bỏ hoàn toàn. Tổng chi phí trở thành sum(b) - sum(a), độc lập với T hợp lệ đã chọn. 
5. Do đó, nếu khả thi, chúng ta tính trực tiếp sum(b) - sum(a) là đáp án. 

Tại sao nó hoạt động xuất phát từ cấu trúc của các hoạt động được phép. Mỗi +1 trong mảng đầu tiên sẽ tăng tổng tổng lên 1 và mỗi -1 trong mảng thứ hai sẽ giảm tổng tổng đi 1. Vì trạng thái cuối cùng buộc cả hai mảng phải bằng cùng một hằng số T nên tổng tổng cuối cùng được cố định ở mức 2nT. Thay đổi ròng cần thiết được xác định hoàn toàn bằng số tiền ban đầu và tính khả thi đảm bảo chúng ta chỉ đi theo những hướng cho phép mà không có mâu thuẫn. Chi phí khớp chính xác với tổng chuyển động đi lên cần thiết trong mảng đầu tiên kết hợp với chuyển động đi xuống ở mảng thứ hai và không có sự tương tác giữa các chỉ số nào làm thay đổi tổng số đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    max_a = max(a)
    min_b = min(b)
    
    if max_a > min_b:
        print(-1)
        return
    
    print(sum(b) - sum(a))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên kiểm tra tính khả thi bằng cách so sánh phần tử lớn nhất trong mảng đầu tiên và phần tử nhỏ nhất trong mảng thứ hai. Điều này buộc phải tồn tại một giá trị mục tiêu chung có thể đạt được từ cả hai hướng mà không vi phạm các ràng buộc hoạt động. 

Khi tính khả thi được xác nhận, câu trả lời sẽ rút gọn thành một biểu thức số học đơn giản. Tổng chênh lệch nắm bắt chính xác tổng số thao tác +1 cần thiết trong mảng đầu tiên và -1 thao tác cần thiết trong mảng thứ hai. Không cần phải khớp hoặc sắp xếp từng phần tử vì mỗi thao tác chỉ ảnh hưởng đến các giá trị cục bộ và không can thiệp vào các giá trị khác. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng sự hội tụ hướng tới mục tiêu T đã chọn và tính tổng các điều chỉnh cho từng phần tử, điều này là không cần thiết và có nguy cơ tràn số nguyên hoặc lỗi sai từng phần một trong xử lý ranh giới. Công thức tính tổng trực tiếp tránh được tất cả những điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
4 5 6
```Chúng tôi tính toán: 

| Bước | tối đa(a) | phút(b) | Khả thi | tổng(a) | tổng (b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 4 | Có | 6 | 15 | - | 
| Tính toán | - | - | Có | - | - | 9 | 

Vì 3  4 nên ta tiến hành. Câu trả lời là tổng(b) - tổng(a) = 15 - 6 = 9. 

Điều này xác nhận rằng tất cả các phần tử có thể được căn chỉnh trong phần chồng chéo và chi phí chỉ phụ thuộc vào sự khác biệt tổng hợp. 

### Ví dụ 2 

đầu vào:```
2
5 6
1 4
```| Bước | tối đa(a) | phút(b) | Khả thi | tổng(a) | tổng (b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 1 | Không | 11 | 5 | - | 

Vì max(a) > min(b), nên không tồn tại mục tiêu chung. Quá trình dừng ngay lập tức và xuất ra -1. 

Điều này chứng tỏ tình trạng không khả thi khi các ràng buộc về hướng xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính cực trị và tổng | 
| Không gian | O(1) | Chỉ tổng hợp được lưu trữ | 

Thuật toán phù hợp thoải mái trong giới hạn vì nó thực hiện số lần quét tuyến tính không đổi trên tối đa 100000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else None
```

```python
# Since standalone execution context is assumed, we redefine solve inline for tests

def solve_test(inp):
    import sys
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    max_a = max(a)
    min_b = min(b)
    
    if max_a > min_b:
        return "-1"
    return str(sum(b) - sum(a))

# provided sample
assert solve_test("""3
1 2 3
4 5 6
""") == "9"

# all equal feasible
assert solve_test("""2
1 1
1 1
""") == "0"

# infeasible gap
assert solve_test("""2
10 10
1 2
""") == "-1"

# single element
assert solve_test("""1
5
9
""") == "4"

# large spread but feasible
assert solve_test("""3
1 100 50
60 200 80
""") == str((60+200+80)-(1+100+50))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | trường hợp nhận dạng chi phí bằng 0 | 
| không khả thi | -1 | lỗi chồng chéo | 
| phần tử đơn | khác biệt trực tiếp | độ đúng ranh giới | 
| phạm vi hỗn hợp | tính toán khác biệt | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các mảng đã giống hệt nhau. Với đầu vào a = [7, 7] và b = [7, 7], max(a) = 7 và min(b) = 7, do đó tính khả thi được giữ nguyên và tổng chênh lệch bằng 0. Thuật toán trả về chính xác 0 mà không cần thực hiện bất kỳ thao tác nào. 

Một trường hợp khó khăn khác là khi tính khả thi hầu như không thất bại. Với a = [5], b = [4], max(a) = 5 và min(b) = 4, do đó thuật toán ngay lập tức đưa ra -1. Bất kỳ nỗ lực tính toán chi phí nào cũng sẽ giả định không chính xác rằng mục tiêu chung tồn tại, nhưng không có số nguyên nào thỏa mãn cả hai ràng buộc định hướng. 

Trường hợp cạnh cuối cùng là giá trị lớn gần bằng 1e9. Vì thuật toán chỉ sử dụng tổng và so sánh nên nó tránh được các vấn đề tràn có thể xuất hiện trong mô phỏng từng phần tử đơn giản của tất cả các mức tăng và giảm.
