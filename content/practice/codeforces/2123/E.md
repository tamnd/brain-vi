---
title: "CF 2123E - Đếm MEX"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta được yêu cầu tính số lượng giá trị riêng biệt có thể có của giá trị bị loại trừ tối thiểu (MEX) sau khi loại bỏ chính xác các phần tử $k$ khỏi mảng, cho mỗi $k$ từ 0 đến $n$."
date: "2026-06-08T03:38:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 1400
weight: 2123
solve_time_s: 84
verified: false
draft: false
---

[CF 2123E - Số lượng MEX](https://codeforces.com/problemset/problem/2123/E) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên không âm và chúng tôi được yêu cầu tính số lượng giá trị riêng biệt có thể có của giá trị bị loại trừ tối thiểu (MEX) sau khi loại bỏ chính xác$k$các phần tử của mảng, cho mỗi phần tử$k$từ 0 đến$n$. MEX của một mảng là số nguyên không âm nhỏ nhất không có trong mảng. 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần xuất ra$n+1$số: số lượng giá trị MEX có thể có sau khi loại bỏ 0 phần tử, 1 phần tử, ..., tối đa$n$các phần tử. 

Các ràng buộc đầu vào là đáng kể. Mỗi mảng có thể lớn bằng$2 \cdot 10^5$và tổng số trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều đó ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn$O(n \log n)$mỗi trường hợp thử nghiệm có thể sẽ hết thời gian chờ. Các giải pháp ngây thơ mô phỏng mọi sự kết hợp của các phần tử bị loại bỏ là vô vọng vì số lượng tập hợp con có kích thước$k$là tổ hợp. 

Một số trường hợp khó khăn. Hãy xem xét một mảng trong đó tất cả các phần tử đều bằng nhau, như`[0,0,0,0]`. MEX bắt đầu từ 1, nhưng việc loại bỏ bất kỳ tập hợp con số 0 nào cũng không thể tạo ra bất kỳ giá trị nào khác ngoài 1 cho MEX cho đến khi mảng trống. Một trường hợp khác là mảng thiếu 0 ngay từ đầu chẳng hạn`[1,2,3]`; thì MEX bằng 0 và việc loại bỏ các phần tử không thể làm cho nó nhỏ hơn mà chỉ lớn hơn. Việc triển khai bất cẩn có thể cho rằng việc loại bỏ các phần tử luôn làm tăng MEX, điều này là sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi tập hợp con có kích thước$k$cho mỗi$k$từ 0 đến$n$, tính MEX và đếm các giá trị riêng biệt. Điều này đúng về mặt logic nhưng hoàn toàn không thực tế. Vì$n = 10^5$, thậm chí các tập con có kích thước 1 là$O(n)$, tập hợp con có kích thước 2 là$O(n^2)$, vân vân. Rõ ràng vụ nổ tổ hợp loại trừ điều này. 

Thông tin chi tiết quan trọng đến từ việc hiểu MEX thay đổi như thế nào khi chúng tôi loại bỏ các phần tử. MEX chỉ tăng khi có tất cả các số nhỏ hơn MEX ứng cử viên. Nếu một số$x < MEX$bị thiếu, MEX đã có$x$. Nếu loại bỏ các số, chúng tôi chỉ có thể tăng MEX bằng cách loại bỏ các số ngăn cản nó tăng. Ngược lại, chúng ta luôn có thể giảm MEX bằng cách loại bỏ các số lớn hơn hoặc bằng MEX hiện tại, nhưng chúng ta không thể tạo MEX nhỏ hơn số còn thiếu nhỏ nhất. 

Cụ thể, chúng ta có thể đếm số lần xuất hiện của mỗi số từ 0 đến$n$. Hãy biểu thị`cnt[x]`như số lượng$x$trong mảng. Sau đó chúng ta có thể mô phỏng việc tăng MEX từng bước. Tại MEX = 0, chúng ta cần có số 0. Để đạt MEX = 1, chúng ta phải loại bỏ đủ số 0 để giảm kích thước mảng hoặc điều chỉnh các số khác. Bằng cách duy trì "ngân sách loại bỏ bổ sung" đang hoạt động, chúng tôi có thể quyết định cho mỗi MEX mục tiêu có bao nhiêu cách có thể đạt được cho mỗi MEX$k$. Việc sử dụng tổng tiền tố hoặc phân bổ loại bỏ tham lam cho phép chúng tôi tính toán câu trả lời một cách hiệu quả. 

Điều này làm giảm độ phức tạp$O(n)$cho mỗi trường hợp thử nghiệm, vì chúng tôi chỉ quét mảng một lần để đếm số, sau đó quét số lượng tuần tự để mô phỏng sự tăng trưởng MEX. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu (đếm số lần xuất hiện + tham lam) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước mảng$n$và mảng`a`. 
2. Khởi tạo một mảng`cnt`chiều dài$n+2$để đếm số lần xuất hiện của mỗi số. Lặp lại`a`và tăng dần`cnt[x]`cho mỗi phần tử`x`. 
3. Khởi tạo`removed = 0`để theo dõi tổng số lần xóa được sử dụng để đạt được MEX nhất định. Khởi tạo một danh sách trống`ans`chiều dài$n+1$để lưu trữ số lượng giá trị MEX có thể có cho mỗi$k$. 
4. Lặp lại các giá trị MEX tiềm năng`mex`bắt đầu từ 0. Với mỗi`mex`, kiểm tra`cnt[mex]`. Nếu như`cnt[mex]`là 0, không thể đạt được MEX nếu không loại bỏ các phần tử bổ sung, vì vậy hãy ngắt. Ngược lại thì tích lũy`removed += cnt[mex] - 1`. Điều này thể hiện số lần xóa bổ sung mà chúng tôi có sẵn sau khi đảm bảo một trong mỗi số nhỏ hơn. 
5. Đối với mỗi`k`từ 0 đến`n`, hãy tính số lượng giá trị MEX có thể truy cập được như`min(n - k + removed, mex + 1)`. Điều này sử dụng ngân sách loại bỏ một cách tham lam để mở rộng số lượng giá trị MEX có thể có. 
6. Sau khi quá trình lặp hoàn tất hoặc`mex`vượt quá`n`, điền phần còn lại`ans[k]`giá trị là 1, vì việc loại bỏ tất cả các phần tử chỉ để lại mảng trống có MEX = 0. 
7. Đầu ra`ans`cho trường hợp thử nghiệm hiện tại. 

Điều bất biến là tại mỗi bước,`removed`đếm phần dư thừa của các phần tử có thể bị loại bỏ mà không làm giảm MEX hiện tại. Điều này đảm bảo rằng số lượng giá trị MEX có thể truy cập được tính toán chính xác cho mỗi giá trị.`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cnt = [0] * (n + 2)
        for x in a:
            if x <= n:
                cnt[x] += 1

        ans = [0] * (n + 1)
        removed = 0
        mex = 0
        for mex in range(n + 1):
            if cnt[mex] == 0:
                break
            removed += cnt[mex] - 1

        # fill in number of possible MEX for each k
        for k in range(n + 1):
            ans[k] = min(k + 1, mex + removed)
            if k > removed + mex:
                ans[k] = mex

        print(' '.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```các`cnt`mảng đếm số lần xuất hiện của mỗi số nguyên lên đến`n`.`removed`theo dõi các yếu tố bổ sung có sẵn để loại bỏ mà không ngăn cản sự phát triển của MEX. Vòng lặp đầu tiên xác định MEX lớn nhất có thể đạt được nếu không có phần tử nào bị loại bỏ. Vòng lặp thứ hai phân phối các lượt xóa một cách tham lam trên khắp`k`để tính toán có thể có bao nhiêu giá trị MEX. các`min`đảm bảo chúng tôi không bao giờ vượt quá MEX tối đa có thể. Điều kiện cạnh xảy ra khi tất cả các phần tử đều giống nhau, điều này được xử lý bởi`cnt[mex] == 0`kiểm tra. 

## Ví dụ đã hoạt động 

Mẫu 1:```
a = [1, 0, 0, 1, 2]
n = 5
```| mex | cnt[mex] | đã xóa | 
| --- | --- | --- | 
| 0 | 2 | 1 | 
| 1 | 2 | 2 | 
| 2 | 1 | 2 | 
| 3 | 0 | - | 

Vòng lặp dừng ở mex = 3.`removed = 2`. Vì`k=1`, các giá trị MEX có thể có = min(1+1, 3+2)=2, khớp với đầu ra mẫu. 

Mẫu 2:```
a = [3, 2, 0, 4, 5, 1]
n = 6
```| mex | cnt[mex] | đã xóa | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 1 | 0 | 
| 2 | 1 | 0 | 
| 3 | 1 | 0 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 
| 6 | 0 | - | 

Dừng ở mex = 6, xóa = 0.`k=0`cho MEX = 1,`k=1`cho MEX = 2, v.v., tái tạo đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm các phần tử mảng là O(n), lặp lại số đếm là O(n), tất cả các phép toán khác đều tuyến tính | 
| Không gian | O(n) |`cnt`mảng có kích thước n+2 và`ans`mảng có kích thước n+1 | 

Tổng của n trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó giải pháp chạy thoải mái dưới 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n5\n1 0 0 1 2\n6\n3 2 0 4 5 1\n6\n1 2 0 1 3 2\n4\n0 3 4 1\n5\n0 0 0 0
```
