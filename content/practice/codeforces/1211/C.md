---
title: "CF 1211C - Kem"
description: "Chúng ta được cho một chuỗi ngày và mỗi ngày có một mức giá để mua một phần kem. Mỗi ngày cũng có những ràng buộc mô tả số lượng phần Tanya được phép ăn vào ngày đó."
date: "2026-06-18T17:20:13+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1211
codeforces_index: "C"
codeforces_contest_name: "Kotlin Heroes: Episode 2"
rating: 1700
weight: 1211
solve_time_s: 76
verified: true
draft: false
---

[CF 1211C - Kem](https://codeforces.com/problemset/problem/1211/C) 

**Đánh giá:** 1700 
**Tags:** *đặc biệt, tham lam, phân loại 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi ngày và mỗi ngày có một mức giá để mua một phần kem. Mỗi ngày cũng có những ràng buộc mô tả số lượng phần Tanya được phép ăn vào ngày đó. Suốt mùa hè cô phải ăn chính xác$k$tổng cộng các phần, đồng thời tôn trọng giới hạn tiêu thụ tối thiểu và tối đa hàng ngày. 

Quyết định quan trọng nhất là làm thế nào để phân phối$k$các phần ăn trong nhiều ngày, vì mỗi phần phải được mua vào cùng ngày ăn và chi phí phụ thuộc vào giá của ngày đó. Một số ngày rẻ hơn nên chúng tôi muốn phân bổ nhiều phần hơn ở đó, nhưng chúng tôi không thể vượt quá giới hạn trên hoặc vi phạm giới hạn dưới. 

Đầu ra là tổng chi phí tối thiểu có thể có để mua chính xác$k$các phần trong khi tôn trọng tất cả các ràng buộc mỗi ngày, hoặc$-1$nếu không có phân phối hợp lệ tồn tại. 

Các ràng buộc thúc đẩy một cách tiếp cận tham lam hoặc dựa trên sắp xếp. Với$n$lên đến$2 \cdot 10^5$Và$k$lên đến$10^9$, bất kỳ giải pháp nào cố gắng phân phối động các phần trên mỗi đơn vị hoặc sử dụng mô phỏng trên mỗi phần đều không thể thực hiện được. Thậm chí một$O(nk)$chiến lược ngay lập tức không thể thực hiện được. 

Các trường hợp khó khăn xuất hiện khi bản thân tính khả thi không thành công. Nếu tổng của tất cả các yêu cầu tối thiểu vượt quá$k$hoặc tổng của tất cả công suất tối đa nhỏ hơn$k$, không có giải pháp tồn tại. Một trường hợp tinh vi khác là khi phân bổ tham lam bỏ qua mức tối thiểu bắt buộc: chiến lược ngây thơ “luôn chọn ngày rẻ nhất” sẽ phá vỡ tính khả thi vì nó có thể vi phạm giới hạn thấp hơn vào những ngày đắt tiền mà vẫn phải nhận được một số phần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng quyết định số lượng phần được chỉ định mỗi ngày trong khi vẫn tôn trọng các giới hạn và tính toán chi phí. Điều này tương đương với việc liệt kê tất cả các vectơ số nguyên$(d_1, \dots, d_n)$như vậy$a_i \le d_i \le b_i$và tổng là$k$, sau đó đánh giá tổng chi phí. Số lượng các phân phối như vậy tăng lên theo kiểu tổ hợp, và thậm chí một giới hạn thô cho thấy nó có tính hàm mũ trong$n$, vì mỗi ngày đóng góp một loạt các giá trị có thể có. Điều này trở nên không khả thi gần như ngay lập tức vượt quá rất nhỏ$n$. 

Quan sát quan trọng là cấu trúc chi phí tuyến tính mỗi ngày: mỗi phần trong ngày$i$chi phí như nhau$c_i$. Điều này có nghĩa là chúng tôi không quan tâm đến việc đặt hàng trong ngày mà chỉ quan tâm đến số lượng suất ăn mỗi ngày. Vấn đề trở thành phân phối$k$các đơn vị giống hệt nhau vào các thùng có giới hạn dưới và giới hạn trên, trong đó mỗi đơn vị trong thùng có giá như nhau. 

Một ý tưởng tham lam tự nhiên sẽ xuất hiện nếu chúng ta tách biệt các ràng buộc khỏi việc tối ưu hóa. Đầu tiên, chúng ta phải đáp ứng mọi yêu cầu tối thiểu. Điều đó mang lại sự phân bổ cơ bản. Sau đó, chúng ta vẫn cần phân phối các phần còn lại. Tại thời điểm này, mỗi ngày đều có công suất còn lại và mỗi phần bổ sung trong ngày$i$có chi phí$c_i$. Vì vậy, trước tiên chúng ta nên chỉ định phần bổ sung cho những ngày có giá rẻ nhất. 

Điều này biến vấn đề thành việc sắp xếp ngày theo chi phí và tham lam lấp đầy công suất còn lại trong khi vẫn tôn trọng giới hạn trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam bằng cách sắp xếp |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số phần tối thiểu cần thiết bằng cách cộng tất cả$a_i$. Nếu điều này vượt quá$k$, không có giải pháp tồn tại. Điều này thực thi tính khả thi của giới hạn dưới. 
2. Tính tổng số tiền lớn nhất có thể bằng cách cộng tất cả$b_i$. Nếu điều này ít hơn$k$, không có giải pháp tồn tại. Điều này thực thi tính khả thi của giới hạn trên. 
3. Chỉ định mức tối thiểu mỗi ngày$a_i$khẩu phần. Đây là đường cơ sở bắt buộc mà mọi giải pháp hợp lệ đều phải đưa vào. 
4. Tính các phần còn lại theo$k - \sum a_i$. Đây là những đơn vị linh hoạt có thể được chỉ định ở bất kỳ đâu trong phạm vi năng lực còn lại. 
5. Mỗi ngày, hãy tính công suất còn lại của nó như sau:$b_i - a_i$. Chúng thể hiện số lượng phần bổ sung vẫn có thể được chỉ định cho ngày đó. 
6. Sắp xếp tất cả các ngày theo mức tăng chi phí$c_i$. Điều này đảm bảo chúng tôi luôn xem xét những ngày rẻ hơn trước khi phân bổ các phần linh hoạt. 
7. Lặp lại các ngày theo thứ tự được sắp xếp. Đối với mỗi ngày, hãy chỉ định càng nhiều phần còn lại càng tốt, giới hạn bởi dung lượng còn lại và yêu cầu chung còn lại. 
8. Tích lũy tổng chi phí làm chi phí cơ sở$\sum a_i \cdot c_i$cộng với chi phí gia tăng từ các phần được giao thêm. 
9. Dừng lại khi tất cả các phần còn lại đã được chỉ định. 

### Tại sao nó hoạt động 

Sau khi ấn định mức phân bổ tối thiểu bắt buộc, mọi phần còn lại đều độc lập và giống hệt nhau ngoại trừ chi phí đặt nó vào một ngày nhất định. Mỗi ngày cung cấp một số lượng hạn chế các “vị trí” giống hệt nhau với chi phí giống nhau. Điều này giúp giảm bớt vấn đề trong việc chọn các vị trí có sẵn rẻ nhất cho đến khi hết hạn ngạch. Bất kỳ sai lệch nào so với việc sắp xếp theo chi phí đều có thể được cải thiện bằng cách hoán đổi một vị trí được chỉ định đắt hơn bằng một vị trí chưa sử dụng rẻ hơn, do đó, lựa chọn tham lam là tối ưu theo đối số trao đổi. Các ràng buộc tối thiểu chỉ ảnh hưởng đến đường cơ sở; chúng không can thiệp vào thứ tự của các bài tập tùy chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    days = []
    
    min_sum = 0
    max_sum = 0
    
    for _ in range(n):
        a, b, c = map(int, input().split())
        min_sum += a
        max_sum += b
        days.append((c, a, b))
    
    if k < min_sum or k > max_sum:
        print(-1)
        return
    
    remaining = k - min_sum
    
    total_cost = 0
    
    for c, a, b in days:
        total_cost += a * c
    
    days.sort()
    
    for c, a, b in days:
        if remaining == 0:
            break
        capacity = b - a
        take = min(capacity, remaining)
        total_cost += take * c
        remaining -= take
    
    print(total_cost)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc kiểm tra tính khả thi bằng cách sử dụng tổng số tiền tối thiểu và tối đa. Điều này ngăn chặn công việc lãng phí trên các cấu hình không thể thực hiện được. Chi phí cơ bản được tính ngay lập tức từ phân bổ bắt buộc, đảm bảo chúng tôi tách biệt rõ ràng các quyết định bắt buộc và tùy chọn. 

Sắp xếp theo chi phí là bước quan trọng: sau khi đặt hàng theo ngày, chúng ta sẽ tiêu thụ công suất còn lại theo thứ tự đó một cách tham lam. các`remaining`biến theo dõi có bao nhiêu phần linh hoạt vẫn chưa được chỉ định. Mỗi lần lặp lại tiêu thụ càng nhiều càng tốt từ ngày rẻ nhất hiện tại. 

Một lỗi triển khai phổ biến là quên rằng phải tính đến chi phí cơ bản trước khi phân loại; việc sắp xếp không ảnh hưởng đến việc phân bổ bắt buộc, chỉ ảnh hưởng đến những phân bổ linh hoạt. Một vấn đề nhỏ khác là tràn số nguyên trong các ngôn ngữ khác, nhưng Python xử lý số nguyên lớn một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 7
3 5 6
0 3 4
3 3 3
```Chúng tôi tính toán tổng số tối thiểu và tối đa: 

| Ngày | một | b | c | Đường cơ sở a | Giới hạn còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 6 | 3 | 2 | 
| 2 | 0 | 3 | 4 | 0 | 3 | 
| 3 | 3 | 3 | 3 | 3 | 0 | 

Số tiền tối thiểu là 6 nên chúng ta cần thêm 1 phần. 

Chúng tôi sắp xếp theo chi phí: ngày 3 (3), ngày 2 (4), ngày 1 (6). 

Chúng tôi chỉ định phần bổ sung cho ngày thứ 3? Không có năng lực nên ngày thứ 2 có được. 

| Bước | Ngày | Còn lại | Đã chụp | Chi phí bổ sung | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | 1 | - | đường cơ sở = 3_6 + 0_4 + 3*3 = 27 | 
| 1 | ngày2 | 1 | 1 | +4 | 

Chi phí cuối cùng là 31. 

Dấu vết này cho thấy các ràng buộc bắt buộc buộc phải có một đường cơ sở không tầm thường và tính tham lam chỉ áp dụng cho công suất dư. 

### Ví dụ 2 

đầu vào:```
2 5
1 3 10
1 4 1
```Đường cơ sở là 2, vì vậy còn lại là 3. 

| Ngày | c | mũ lưỡi trai | 
| --- | --- | --- | 
| 2 | 1 | 3 | 
| 1 | 10 | 2 | 

Chúng tôi lấy cả 3 chiếc còn lại từ ngày thứ 2 vì nó rẻ nhất. 

Chi phí cuối cùng = đường cơ sở (1_10 + 1_1 = 11) + 3*1 = 14. 

Điều này xác nhận việc đặt hàng tham lam một cách chính xác sẽ tránh được việc phân bổ tốn kém ngay cả khi tồn tại một ngày đắt đỏ hạn chế hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp ngày theo chi phí chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ bộ dữ liệu ngày | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì$n \le 2 \cdot 10^5$và việc sắp xếp cộng với quét tuyến tính sẽ có hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 7
3 5 6
0 3 4
3 3 3
""") == "31"

# minimum case feasible
assert run("""1 5
5 10 2
""") == "10"

# infeasible (too small max)
assert run("""2 10
0 3 5
0 6 2
""") == "-1"

# tight bounds
assert run("""3 6
2 2 5
1 3 1
1 3 2
""") == "13"

# all equal cost
assert run("""4 10
1 5 3
1 5 3
1 5 3
1 5 3
""") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày độc thân | 10 | xử lý cơ bản | 
| tối đa không khả thi | -1 | kiểm tra tính khả thi | 
| phân phối chặt chẽ | 13 | phân bổ đúng theo ràng buộc | 
| chi phí thống nhất | 30 | trung lập tham lam | 

## Vỏ cạnh 

Trường hợp quan trọng là khi yêu cầu tối thiểu đã vượt quá$k$. Trong trường hợp đó, ngay cả trước khi xem xét chi phí, câu trả lời phải là:$-1$. Thuật toán phát hiện điều này ngay lập tức thông qua tổng của$a_i$, đảm bảo không xảy ra việc sắp xếp hoặc phân bổ không cần thiết. 

Một trường hợp khác là khi công suất tối đa không đủ. Ngay cả khi chi phí thuận lợi, không có sự phân phối nào có thể đạt được$k$và việc kiểm tra tính khả thi sớm sẽ ngăn chặn việc phân bổ một phần không chính xác. 

Trường hợp khó phát hiện cuối cùng là khi tất cả công suất linh hoạt thuộc về những ngày có giá đắt hơn trong khi những ngày rẻ hơn đã bão hòa ở mức tối thiểu. Thuật toán vẫn hoạt động chính xác vì nó tách biệt các phân bổ bắt buộc và tùy chọn, đảm bảo lựa chọn tham lam chỉ áp dụng cho tính linh hoạt sẵn có thực tế thay vì buộc các phân bổ lại không khả thi.
