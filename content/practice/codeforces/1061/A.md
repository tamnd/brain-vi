---
title: "CF 1061A - Tiền xu"
description: "Chúng tôi được cấp một tập hợp các mệnh giá tiền xu bao gồm mọi giá trị nguyên từ 1 đến n và chúng tôi được phép sử dụng bất kỳ số lượng xu nào thuộc bất kỳ giá trị nào trong số này."
date: "2026-06-15T08:50:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1061
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 523 (Div. 2)"
rating: 800
weight: 1061
solve_time_s: 118
verified: true
draft: false
---

[CF 1061A - Tiền xu](https://codeforces.com/problemset/problem/1061/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tập hợp các mệnh giá tiền xu bao gồm mọi giá trị nguyên từ 1 đến n và chúng tôi được phép sử dụng bất kỳ số lượng xu nào thuộc bất kỳ giá trị nào trong số này. Nhiệm vụ là biểu diễn tổng mục tiêu S bằng cách sử dụng càng ít đồng xu càng tốt, trong đó việc lặp lại cùng một giá trị đồng xu được cho phép mà không bị hạn chế. 

Đây thực chất là một bài toán giảm thiểu tài nguyên: chúng ta muốn phân tách S thành nhiều tập hợp số nguyên trong phạm vi [1, n], giảm thiểu kích thước của tập hợp đó. Mỗi đồng xu đóng góp giá trị của nó vào tổng số và chúng tôi muốn đạt được chính xác S mà không vượt quá nó. 

Các ràng buộc rất quan trọng ở đây. Giá trị của n có thể lớn tới 100000, trong khi S có thể lớn tới 1e9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các tổ hợp hoặc lập trình động trên S, vì bất kỳ không gian trạng thái nào phụ thuộc vào S đều sẽ quá lớn. Ngay cả giải pháp O(S) cũng không thể thực hiện được. Chúng tôi buộc phải sử dụng một giải pháp tính toán câu trả lời theo thời gian không đổi hoặc thời gian logarit cho mỗi trường hợp thử nghiệm, dựa vào cấu trúc thay vì liệt kê. 

Một sai lầm ngây thơ nhưng phổ biến là cho rằng việc sử dụng đồng tiền lớn nhất một cách tham lam luôn có tác dụng bằng cách liên tục lấy min(n, S) cho đến khi S bằng 0. Điều này thất bại một cách tinh vi khi phần còn lại sau khi lấy những đồng tiền lớn là nhỏ nhưng không thể thể hiện một cách hiệu quả nếu không tăng tổng số đồng xu. Một trực giác sai lầm khác là coi nó như một vấn đề về chiếc ba lô và cố gắng trực tiếp tối ưu hóa bố cục; điều này cũng thất bại do quy mô của S. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi tập hợp xu có thể có có giá trị trong [1, n] và tổng của chúng là S, rồi tính số lượng quân số tối thiểu. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các phân rã hợp lệ. Tuy nhiên, số lượng nhiều tập hợp như vậy tăng cực kỳ nhanh chóng với S. Ngay cả khi giới hạn ở các giá trị đồng xu bị chặn, số phân vùng của một số nguyên vẫn là hàm mũ trong căn bậc hai của S trong cài đặt chung, khiến phương pháp này không thể thực hiện được từ rất lâu trước khi đạt đến các ràng buộc nhất định. 

Quan sát quan trọng là cấu trúc của bộ tiền xu làm cho vấn đề hoạt động giống như một phiên bản ràng buộc của phân vùng số nguyên trong đó các giá trị lớn luôn được ưu tiên hơn. Nếu chúng ta sử dụng k xu, số tiền tốt nhất có thể đạt được với k xu là thu được bằng cách lấy k bản sao của n, cho k·n. Điều này có nghĩa là bất kỳ giải pháp nào sử dụng k xu chỉ có thể bao gồm số tiền lên tới k·n. Ngược lại, nếu chúng ta muốn đạt được S, chúng ta cần ít nhất đủ số xu để tổng số tiền tối đa có thể đạt được của chúng đạt đến S. 

Điều này ngay lập tức gợi ý giới hạn dưới của trần(S / n). Câu hỏi sâu hơn là liệu giới hạn này có luôn đạt được hay không. Hóa ra là như vậy, bởi vì chúng ta luôn có thể biểu thị bất kỳ số dư nào bằng cách sử dụng các giá trị đồng xu nhỏ hơn: khi chúng ta lấy càng nhiều n đồng càng tốt, thì phần còn lại hoàn toàn nhỏ hơn n và phần còn lại luôn có thể được biểu thị bằng một đồng xu duy nhất (hoặc nhiều nhất là một đồng xu bổ sung trong các điều chỉnh kết hợp, nhưng cấu trúc đảm bảo tính khả thi). Điều này làm cho giới hạn dưới chặt chẽ. 

Vì vậy, chiến lược tối ưu rút gọn thành một đối số phân chia đơn giản: sử dụng càng nhiều đồng xu có giá trị n càng tốt và nếu còn lại, nó có thể được bao phủ bằng một đồng xu bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào việc so sánh tổng mục tiêu S với sự đóng góp tối đa của đồng tiền có kích thước n.

1. Tính xem có bao nhiêu đồng xu cỡ n vừa với S. Đây là S chia cho n bằng phép chia số nguyên. Điều này thể hiện số lượng tiền lớn tối đa mà chúng tôi có thể sử dụng mà không vượt quá S. 
2. Tính số dư sau khi lấy những đồng tiền cỡ n đó. Đây là S modulo n. Phần còn lại là số tiền còn lại không thể bù đắp bằng n xu đầy đủ. 
3. Nếu số dư bằng 0, câu trả lời chính xác là số đồng xu đầy đủ, vì S chia hết cho n. 
4. Nếu số dư khác 0 thì chúng ta cần thêm một đồng xu để che phần còn lại. Điều này là do mọi giá trị từ 1 đến n đều có sẵn, do đó mọi phần dư nhỏ hơn n luôn có thể được hình thành bằng cách sử dụng một đồng xu. 

Lý do xoay quanh thực tế là giá trị đồng xu 1 luôn tồn tại, do đó, mọi phần còn lại luôn có thể được biểu diễn mà không làm tăng thêm độ phức tạp. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi lấy các đồng xu sàn (S / n) có giá trị n, giá trị còn lại hoàn toàn nhỏ hơn n và do đó luôn có thể biểu diễn bằng cách sử dụng bộ đồng xu có sẵn. Vì chúng tôi luôn ưu tiên những đồng tiền lớn hơn để giảm thiểu số lượng, nên mọi giải pháp tối ưu đều phải sử dụng càng nhiều n đồng tiền càng tốt; thay thế một đồng xu n bằng những đồng xu nhỏ hơn không bao giờ làm giảm số lượng đồng xu cần thiết. Điều này đảm bảo rằng việc phân hủy tham lam thành các khối có kích thước n cộng với phần còn lại là tối ưu và không thể cải thiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    ans = s // n
    if s % n != 0:
        ans += 1
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện lý luận dựa trên phép chia. Phép chia số nguyên tính toán có bao nhiêu n đồng xu đầy đủ phù hợp với S. Phần tăng có điều kiện xử lý mọi phần còn lại cần thêm một đồng xu. 

Một điểm tinh tế là chúng ta không cần phải xây dựng bộ tiền xu thực tế. Bài toán chỉ yêu cầu số lượng xu tối thiểu chứ không yêu cầu thành phần nên số học là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 5, S = 11 

| Bước | Toàn xu (S//n) | Phần còn lại (S%n) | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 

Chúng ta có thể lấy hai đồng xu giá trị 5, cho 10. 1 đồng còn lại cần thêm một đồng xu. Điều này xác nhận rằng 3 xu là đủ. 

### Ví dụ 2 

Đầu vào: n = 6, S = 16 

| Bước | Toàn xu (S//n) | Phần còn lại (S%n) | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 3 | 

Hai đồng xu có giá trị 6 cho 12, còn lại 4. Một đồng xu bổ sung bao gồm phần còn lại. 

Những dấu vết này cho thấy thuật toán đang phân tách S thành các khối tối đa có kích thước n một cách hiệu quả và xử lý phần còn lại một cách độc lập, xác nhận tính đúng đắn của hành vi phân chia trần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì nó thực hiện một số phép toán số nguyên không đổi ngay cả khi S lớn bằng 1e9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, s = map(int, input().split())
    ans = s // n
    if s % n != 0:
        ans += 1
    return str(ans)

# provided samples
assert run("5 11\n") == "3", "sample 1"
assert run("6 16\n") == "3", "sample 2"

# custom cases
assert run("1 1000000000\n") == "1000000000", "only coin 1 available"
assert run("10 10\n") == "1", "exact division"
assert run("10 1\n") == "1", "small remainder"
assert run("7 49\n") == "7", "large exact multiple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1000000000 | 1000000000 | trường hợp cực đoan khi chỉ có đồng xu đơn vị mới quan trọng | 
| 10 10 | 1 | chia hết chính xác | 
| 10 1 | 1 | trường hợp còn lại nhỏ nhất | 
| 7 49 | 7 | tính nhất quán chính xác lớn hơn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi S nhỏ hơn n. Trong tình huống này, S // n trở thành 0 và S % n bằng S. Thuật toán trả về chính xác 1, vì bất kỳ giá trị nào lên đến n đều có thể được biểu thị bằng một đồng xu. Ví dụ: với n = 10 và S = 7, chúng ta lấy 0 đồng xu đầy đủ và một đồng xu có giá trị 7. 

Một trường hợp cạnh khác là khi S chia hết cho n. Ví dụ: n = 4 và S = 12 cho S // n = 3 và số dư 0, do đó không có thêm xu nào được thêm vào. Điều này phản ánh chiến lược tối ưu là chỉ sử dụng những đồng tiền có giá trị tối đa. 

Trường hợp cạnh cuối cùng là khi n = 1. Ở đây mỗi đồng xu là 1, vì vậy cách duy nhất để tạo thành S là sử dụng S đồng xu. Công thức cho S // 1 = S và dư 0, khớp chính xác với câu trả lời đúng.
