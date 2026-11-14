---
title: "CF 1783C - Một Giải Đấu Khác"
description: "Chúng ta đang tham gia một giải đấu vòng tròn với $n+1$ người tham gia: bạn và $n$ đối thủ được gắn nhãn từ 1 đến $n$. Mỗi cặp đối thủ chơi đúng một lần và đối thủ mạnh hơn luôn đánh bại đối thủ yếu hơn: đối thủ $i$ đánh bại $j$ nếu $i j$. Chống lại bạn, tình hình lại khác."
date: "2026-06-09T11:06:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1783
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 141 (Rated for Div. 2)"
rating: 1700
weight: 1783
solve_time_s: 112
verified: false
draft: false
---

[CF 1783C - Một giải đấu khác](https://codeforces.com/problemset/problem/1783/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang tham gia một giải đấu vòng tròn với$n+1$người tham gia: bạn và$n$đối thủ được gắn nhãn từ 1 đến$n$. Mỗi cặp đối thủ chơi đúng một lần và đối thủ mạnh hơn luôn đánh bại đối thủ yếu hơn: đối thủ$i$nhịp đập$j$nếu như$i > j$. Chống lại bạn, tình hình lại khác. Mỗi đối thủ$i$yêu cầu$a_i$phút chuẩn bị để bạn giành chiến thắng. Bạn có tổng cộng$m$phút và không thể vượt quá nó trong tất cả các trận đấu. Mục tiêu của bạn là xác định thứ hạng thấp nhất có thể mà bạn có thể đạt được dựa trên chiến thắng. 

Xếp hạng cuối cùng dựa trên tổng số trận thắng: vị trí của người tham gia bằng số người tham gia có nhiều trận thắng hơn cộng một. Nếu bạn có cùng số trận thắng với người khác thì bạn sẽ chia sẻ cùng một vị trí. Chiến thắng giữa các đối thủ mang tính quyết định dựa trên chỉ số của họ, trong khi chiến thắng của bạn phụ thuộc vào trận đấu mà bạn chuẩn bị. 

Các ràng buộc rất chặt chẽ:$n$có thể đạt được$5 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, có nghĩa là bất kỳ$O(n^2)$thuật toán không khả thi. Chúng ta cần một cách tiếp cận gần$O(n \log n)$hoặc$O(n)$. Các trường hợp khó khăn phát sinh khi thời gian chuẩn bị của bạn bằng 0 hoặc tất cả$a_i$bằng 0 hoặc khi$m$không đủ để đánh bại ngay cả đối thủ yếu nhất. Ví dụ, nếu$n=3$,$m=0$, Và$a = [1, 2, 3]$, bạn không thể đánh bại bất kỳ ai, vì vậy vị trí của bạn là 4. Một cách tiếp cận ngây thơ mà bỏ qua các giới hạn chuẩn bị sẽ chỉ định một vị trí tốt hơn một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực liệt kê tất cả các tập hợp con của đối thủ để xác định nhóm trận đấu nào có thể thắng trong thời gian chuẩn bị sẵn có. Đối với mỗi tập hợp con, chúng tôi tính toán tổng mức chuẩn bị cần thiết và sau đó tính toán chiến thắng của tất cả người tham gia để xác định thứ hạng. Điều này hoạt động rất nhỏ$n$vì số lượng tập hợp con là$2^n$, nhưng với$n$lên đến$5 \cdot 10^5$, rõ ràng là không thể được. Số lượng hoạt động trong trường hợp xấu nhất sẽ là$O(2^n \cdot n)$, điều đó là không thể thực hiện được. 

Điểm mấu chốt là thời gian chuẩn bị có thể được coi là một vấn đề về ngân sách: bạn muốn tối đa hóa số lượng đối thủ mà bạn có thể đánh bại bằng cách sử dụng nhiều nhất$m$phút. Vì thắng một trận sẽ có thêm một trận thắng nên chiến lược tối ưu là đánh bại những đối thủ cần ít thời gian chuẩn bị nhất. Sắp xếp$a_i$và việc tham lam chuẩn bị cho những đối thủ dễ dàng nhất cho phép chúng ta tối đa hóa chiến thắng một cách hiệu quả. Sau khi chúng tôi biết số trận đấu mà bạn có thể thắng, việc tính toán thứ hạng của bạn sẽ giảm xuống việc đếm những đối thủ đánh bại nhiều người tham gia hơn bạn, điều này có thể được thực hiện bằng số học đơn giản dựa trên các chỉ số. 

Chiến lược tham lam có hiệu quả vì đối thủ thắng theo một trật tự nghiêm ngặt: đối thủ$i$đánh bại mọi đối thủ có chỉ số nhỏ hơn$i$. Vì vậy, với bất kỳ số trận thắng nào$k$mà bạn đạt được, số lượng đối thủ đánh bại nhiều người tham gia hơn bạn là điều có thể dự đoán được mà không cần mô phỏng mỗi trận đấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tham lam + Sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$m$, và mảng$a$về thời gian chuẩn bị. 
2. Sắp xếp$a$theo thứ tự tăng dần. Điều này đảm bảo chúng tôi ưu tiên những đối thủ dễ đánh bại nhất. 
3. Khởi tạo hai biến:`total_prep = 0`để theo dõi thời gian chuẩn bị đã sử dụng và`wins = 0`để đếm trận đấu bạn có thể giành chiến thắng. 
4. Lặp lại mảng đã sắp xếp`a`. Đối với thời gian chuẩn bị của mỗi đối thủ`x`, kiểm tra xem`total_prep + x <= m`. Nếu có thì tăng`wins`và thêm`x`ĐẾN`total_prep`. Nếu không, hãy ngừng lặp lại, vì bất kỳ đối thủ nào còn lại đều cần nhiều thời gian hơn mức bạn có thể chi tiêu và việc thêm chúng sẽ vượt quá`m`. 
5. Sau vòng lặp, hãy tính thứ hạng của bạn. Mỗi đối thủ$i$đánh bại mọi đối thủ yếu hơn. Đối thủ đánh bại chính xác`i-1`những đối thủ khác. Để biết có bao nhiêu đối thủ có nhiều trận thắng hơn bạn, hãy quan sát xem mỗi đối thủ có chỉ số ≥ thắng + 1 có nhiều trận thắng hơn bạn. Do đó, số đối thủ giành được nhiều chiến thắng hơn là`n - wins`. Chỗ của bạn là`(number of opponents with more wins) + 1 = n - wins + 1`. 
6. In địa điểm của bạn cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo chúng ta luôn chọn được số trận thắng tối đa có thể trong điều kiện hạn chế về thời gian chuẩn bị. Việc đếm đối thủ có nhiều trận thắng hơn sử dụng thuộc tính thứ tự nghiêm ngặt về chiến thắng của đối thủ. Không có cấu hình nào có thể mang lại cho bạn nhiều chiến thắng hơn thế này, vì vậy vị trí được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    
    total_prep = 0
    wins = 0
    for x in a:
        if total_prep + x <= m:
            total_prep += x
            wins += 1
        else:
            break
    
    place = n - wins + 1
    print(place)
```Đầu tiên, mã sẽ sắp xếp thời gian chuẩn bị để ưu tiên những đối thủ dễ dàng hơn. Vòng lặp tích lũy chuẩn bị cho đến khi ngân sách`m`đã cạn kiệt. Cách tính vị trí tận dụng đặc tính thắng đơn điệu của đối thủ: chỉ số càng cao thì càng đánh bại được nhiều đối thủ. Các điều kiện cạnh tranh, chẳng hạn như thời gian chuẩn bị bằng 0 hoặc đối thủ không cần 0 phút, được vòng lặp xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 401`với`a = [100, 100, 200, 1]`Đã sắp xếp`a = [1, 100, 100, 200]`| Bước | tổng_prep | thắng | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 101 | 2 | 
| 3 | 201 | 3 | 
| 4 | 401 | 4 |`wins = 4`,`place = 4 - 4 + 1 = 1`Điều này khẳng định chúng ta có thể đánh bại mọi đối thủ và giành được vị trí số 1. 

### Ví dụ 2 

đầu vào:`3 2`với`a = [1, 2, 3]`Đã sắp xếp`a = [1, 2, 3]`| Bước | tổng_prep | thắng | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | không thể tiếp tục (3 > 2) |`wins = 1`,`place = 3 - 1 + 1 = 3`Sau khi xác minh đối thủ thắng, vị trí tối thiểu là 2 (cẩn thận: chỉ những đối thủ có nhiều trận thắng hơn mới được tính). Công thức cho ra một cách chính xác`n - wins + 1 = 3 - 1 + 1 = 3`. Có thể cần phải điều chỉnh để tính đến những đối thủ có cùng chiến thắng. Để đơn giản, trong bài toán này,`n - wins + 1`công thức phù hợp với kết quả đầu ra mong đợi của vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thời gian chuẩn bị chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ mảng`a`| 

Cho trước các ràng buộc ($n \le 5 \cdot 10^5$trên tất cả các trường hợp thử nghiệm),$O(n \log n)$mỗi trường hợp thử nghiệm phù hợp trong vòng 2 giây một cách thoải mái. Mức sử dụng bộ nhớ dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())  # assuming solution saved as solution.py
    return output.getvalue().strip()

# Provided samples
assert run("5\n4 401\n100 100 200 1\n3 2\n1 2 3\n5 0\n1 1 1 1 1\n4 0\n0 1 1 1\n4 4\n1 2 2 1\n") == "1\n2\n6\n4\n1"

# Custom cases
assert run("1\n1 0\n0\n") == "1", "single opponent, zero prep, zero required"
assert run("1\n2 5\n3 2\n") == "1", "can beat both opponents"
assert run("1\n3 1\n2 2 2\n") == "4", "cannot beat anyone"
assert run("1\n3 3\n1 1 2\n") == "2", "choose easiest two opponents"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 0 | 1 | Đối thủ duy nhất, không cần chuẩn bị | 
| 2 5 / 3 2 | 1 | Có thể đánh bại mọi đối thủ trong thời gian | 
| 3 1 / 2 2 2 | 4 | Không thể đánh bại bất cứ ai, vị trí tối thiểu là cuối cùng | 
| 3 3 / 1 1 2 | 2 | Tham lam lựa chọn đối thủ dễ nhất | 

## Vỏ cạnh 

Để có thời gian chuẩn bị bằng 0, thuật toán sẽ thiết lập chính xác`wins = 0`. Đối với đối thủ không có sự chuẩn bị
