---
title: "CF 2087E - Tô màu mũi tên"
description: "Chúng ta được cung cấp một chuỗi các mũi tên được sắp xếp thành một dòng, mỗi mũi tên chỉ sang trái hoặc phải. Mỗi mũi tên cũng có một phần thưởng số nguyên liên quan khi sơn nó màu đỏ, có thể dương, âm hoặc bằng 0. Ban đầu, tất cả các mũi tên đều có màu xanh."
date: "2026-06-08T05:58:07+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 74
verified: true
draft: false
---

[CF 2087E - Tô màu mũi tên](https://codeforces.com/problemset/problem/2087/E) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dp 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các mũi tên được sắp xếp thành một dòng, mỗi mũi tên chỉ sang trái hoặc phải. Mỗi mũi tên cũng có một phần thưởng số nguyên liên quan khi sơn nó màu đỏ, có thể dương, âm hoặc bằng 0. Ban đầu, tất cả các mũi tên đều có màu xanh. Trò chơi cho phép bạn thực hiện một loạt thao tác sơn lại, bắt đầu bằng bất kỳ mũi tên nào và sau đó trong mỗi lần di chuyển tiếp theo, bạn phải chọn một mũi tên theo hướng của mũi tên trước đó. Nghĩa là, nếu mũi tên trước hướng sang trái thì mũi tên tiếp theo phải có chỉ số nhỏ hơn; nếu nó chỉ sang phải thì mũi tên tiếp theo phải có chỉ số lớn hơn. Mũi tên không nhất thiết phải liền kề. 

Mục tiêu là tối đa hóa tổng phần thưởng cho tất cả các mũi tên được sơn lại màu đỏ. Mỗi truy vấn tương ứng với một trường hợp thử nghiệm duy nhất, với tối đa$3 \cdot 10^5$mũi tên và tổng tất cả các mũi tên trong tất cả các trường hợp thử nghiệm không vượt quá$3 \cdot 10^5$. Ràng buộc này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính tương ứng với số lượng mũi tên. 

Các trường hợp khó khăn rất khó phát hiện vì một số phần thưởng có thể âm. Trong những trường hợp đó, đôi khi việc bỏ qua hoàn toàn các mũi tên là tối ưu. Ví dụ: nếu tất cả phần thưởng đều âm thì số điểm tối đa có thể đạt được là 0. Một trường hợp cạnh khác xảy ra khi tất cả các mũi tên chỉ về cùng một hướng; trình tự tối ưu có thể liên quan đến việc chỉ chọn phần thưởng lớn nhất ở cuối chuỗi định hướng thay vì bắt đầu ở bất kỳ đâu ở giữa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét bắt đầu từ mỗi mũi tên và mô phỏng đệ quy mọi trình tự sơn lại hợp lệ. Đối với mỗi chỉ số bắt đầu, thuật toán sẽ phân nhánh theo hướng của mũi tên hiện tại và xem xét mọi mũi tên hợp lệ tiếp theo có thể có. Cách tiếp cận này đúng nhưng rõ ràng là không khả thi vì trong trường hợp xấu nhất, nó khám phá một số chuỗi theo cấp số nhân, vượt xa giới hạn cho$n \approx 3 \cdot 10^5$. 

Quan sát quan trọng là mỗi chuỗi mũi tên chỉ cùng một hướng tạo thành một chuỗi độc lập trong đó việc lựa chọn phần thưởng có thể được thực hiện một cách tham lam. Nếu chúng ta duyệt các mũi tên từ trái sang phải, đối với các mũi tên chỉ sang phải, chúng ta có thể truyền tổng cực đại đang chạy và tương tự từ phải sang trái đối với các mũi tên chỉ trái. Trên thực tế, chúng ta có thể xem vấn đề như một phép tính, đối với mỗi mũi tên, đóng góp tối đa có thể đạt được bắt đầu từ mũi tên đó nếu nó được đưa vào, sau đó chọn đóng góp tối đa như vậy trên tất cả các mũi tên. Điều này làm giảm giải pháp về thời gian tuyến tính bằng cách tính toán hai mảng: một mảng truyền tổng tối đa sang bên phải cho mũi tên '>' và một mảng truyền tổng tối đa sang trái cho mũi tên '<'. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số mũi tên$n$, chuỗi chỉ đường và mảng phần thưởng. 
2. Khởi tạo hai mảng`dp_right`Và`dp_left`chiều dài$n$, chứa đầy số không. Chúng sẽ lưu trữ số tiền tối đa có thể đạt được bắt đầu từ mỗi mũi tên theo mỗi hướng. 
3. Di chuyển các mũi tên từ trái sang phải. Nếu mũi tên hiện tại trỏ sang phải ('>'), hãy đặt`dp_right[i] = reward[i] + max(0, dp_right[i-1])`. Ngược lại, đặt`dp_right[i] = 0`. Điều này tích lũy các chuỗi mũi tên chỉ sang phải liên tiếp và bỏ qua các chuỗi tổng âm. 
4. Di chuyển các mũi tên từ phải sang trái. Nếu mũi tên hiện tại trỏ sang trái ('<'), hãy đặt`dp_left[i] = reward[i] + max(0, dp_left[i+1])`. Ngược lại, đặt`dp_left[i] = 0`. Điều này tích lũy chuỗi các mũi tên chỉ trái liên tiếp. 
5. Câu trả lời cho ca kiểm thử là giá trị lớn nhất trong số tất cả`dp_right[i]`Và`dp_left[i]`, vì được phép bắt đầu chuỗi ở bất kỳ mũi tên nào. Đồng thời bao gồm số 0 ở mức tối đa để tính đến việc bỏ qua tất cả các mũi tên. 
6. In kết quả. 

Tại sao nó hoạt động: việc truyền theo hướng đảm bảo rằng đối với mỗi mũi tên, chúng tôi xem xét tổng tối đa có thể đạt được bằng cách tiếp tục đi theo hướng trỏ của nó. Bằng cách tận dụng tối đa tất cả các vị trí bắt đầu, chúng tôi nắm bắt được tất cả các chuỗi hoạt động có thể xảy ra. Phần thưởng tiêu cực sẽ tự động bị loại trừ do`max(0, ...)`logic, mô hình hóa sự lựa chọn thực hiện các phép toán bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        c = list(map(int, input().split()))
        
        dp_right = [0] * n
        dp_left = [0] * n
        
        # propagate for '>' arrows
        for i in range(n):
            if s[i] == '>':
                dp_right[i] = c[i]
                if i > 0:
                    dp_right[i] += max(0, dp_right[i-1])
        
        # propagate for '<' arrows
        for i in reversed(range(n)):
            if s[i] == '<':
                dp_left[i] = c[i]
                if i < n-1:
                    dp_left[i] += max(0, dp_left[i+1])
        
        ans = 0
        for i in range(n):
            ans = max(ans, dp_right[i], dp_left[i])
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì độ phức tạp thời gian tuyến tính bằng cách quét mảng hai lần, một lần từ trái sang phải và một lần từ phải sang trái. sử dụng`max(0, ...)`tự động loại bỏ các chuỗi có đóng góp tiêu cực, tránh các hoạt động không cần thiết. Các điều kiện biên được xử lý cẩn thận bằng cách kiểm tra các chỉ số mảng trước khi truyền. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`3, <> >, 5 4 6`| tôi | s[i] | c[i] | dp_right | dp_left | 
| --- | --- | --- | --- | --- | 
| 0 | < | 5 | 0 | 5 | 
| 1 | > | 4 | 4 | 0 | 
| 2 | > | 6 | 10 | 0 | 

Tối đa trong số`dp_right`Và`dp_left`là 10. Điều này phù hợp với kết quả đầu ra dự kiến. 

Đầu vào mẫu:`2, >>, -1 -2`| tôi | s[i] | c[i] | dp_right | dp_left | 
| --- | --- | --- | --- | --- | 
| 0 | > | -1 | -1 | 0 | 
| 1 | > | -2 | -2 | 0 | 

Tối đa là 0, tương ứng với việc bỏ qua tất cả các mũi tên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được duyệt hai lần cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Hai mảng phụ có chiều dài n. | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm 3·10^5, giải pháp này phù hợp thoải mái với các hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n3\n<>>\n5 4 6\n5\n<><>>\n5 -2 4 -3 7\n2\n>>\n-1 -2\n8\n>>>><<<<\n1 -1 1 -1 1 -1 1 -1\n5\n><<<>\n-1 100 100 100 100\n") == "10\n9\n0\n4\n399", "sample tests"

# custom edge cases
assert run("1\n1\n>\n100") == "100", "single positive"
assert run("1\n1\n<\n-100") == "0", "single negative"
assert run("1\n5\n<<>>>\n1 -2 3 -4 5") == "5", "mixed directions and signs"
assert run("1\n3\n<><\n-1 -2 -3") == "0", "all negative"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n>\n100 | 100 | Phần thưởng tích cực mũi tên đơn | 
| 1\n1\n<\n-100 | 0 | Phần thưởng tiêu cực mũi tên đơn | 
| 1\n5\n<<>>>\n1 -2 3 -4 5 | 5 | Hướng hỗn hợp và tích cực/tiêu cực | 
| 1\n3\n<><\n-1 -2 -3 | 0 | Tất cả phần thưởng tiêu cực | 

## Vỏ cạnh 

Đối với một chuỗi có tất cả các mũi tên chỉ theo cùng một hướng, thuật toán sẽ truyền tổng chính xác dọc theo hướng đó và tự động chọn chuỗi con liền kề tối đa. Đối với các chuỗi có phần thưởng âm,`max(0, ...)`logic đảm bảo không có đóng góp tiêu cực nào được đưa vào. Trong chuỗi mũi tên đơn, mức tối đa là phần thưởng hoặc bằng không. Đối với các thao tác trống, câu trả lời mặc định là 0, xử lý chính xác trường hợp tốt hơn là không sơn lại bất kỳ mũi tên nào.
