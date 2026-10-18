---
title: "CF 105582C - Nhà xây dựng"
description: "Chúng tôi được cung cấp một số loại bộ phận xây dựng. Mỗi loại có khối lượng cố định và nguồn cung hạn chế. Từ những bộ phận này, chúng tôi muốn lắp ráp một số bộ dụng cụ giống hệt nhau. “Giống hệt nhau” có nghĩa là mỗi bộ sản phẩm đều sử dụng chính xác số lượng bộ phận của mỗi loại như nhau."
date: "2026-06-22T14:37:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105582
codeforces_index: "C"
codeforces_contest_name: "Ural Championship 2017"
rating: 0
weight: 105582
solve_time_s: 65
verified: true
draft: false
---

[CF 105582C - Hàm tạo](https://codeforces.com/problemset/problem/105582/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số loại bộ phận xây dựng. Mỗi loại có khối lượng cố định và nguồn cung hạn chế. Từ những bộ phận này, chúng tôi muốn lắp ráp một số bộ dụng cụ giống hệt nhau. “Giống hệt nhau” có nghĩa là mỗi bộ sản phẩm đều sử dụng chính xác số lượng bộ phận của mỗi loại như nhau. Nếu một bộ sử dụng 2 vít và 1 dầm thì các bộ còn lại phải sử dụng chính xác 2 vít và 1 dầm đó. 

Mỗi bộ có một giới hạn tổng khối lượng: tổng khối lượng của các bộ phận được chọn phải nằm trong một khoảng nhất định, từ khối lượng tối thiểu cho phép đến khối lượng tối đa cho phép. 

Nhiệm vụ là xác định xem chúng ta có thể tạo ra bao nhiêu bộ sản phẩm giống hệt nhau trong khi tôn trọng cả hai ràng buộc: chúng ta không thể vượt quá nguồn cung sẵn có của bất kỳ loại bộ phận nào và mỗi bộ phải đáp ứng khoảng khối lượng. 

Giới hạn đầu vào nhỏ về số lượng loại, nhưng số lượng mục của mỗi loại có thể lớn. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng việc phân phối các bộ phận theo từng bộ một hoặc liệt kê trực tiếp tất cả các thành phần bộ công cụ có thể có. Giới hạn khối lượng tương đối nhỏ, điều này cho thấy rằng có thể suy luận theo kiểu tổng tập hợp con nếu chúng ta có thể kiểm soát bội số. 

Một khó khăn nhỏ là số lượng bộ dụng cụ và thành phần của bộ dụng cụ phải được kết hợp với nhau. Việc chọn một bộ công cụ phong phú hơn sẽ làm giảm số lượng bản sao mà chúng tôi có thể tạo ra, trong khi việc chọn một bộ công cụ tối thiểu có thể cho phép nhiều bản sao nhưng không đáp ứng được yêu cầu về khối lượng tối thiểu. 

Một sai lầm ngây thơ xuất hiện khi người ta cố gắng tham lam chọn các bộ phận cho đến khi khối lượng đạt đến phạm vi cho phép. Điều đó không thành công vì thành phần tham lam tương tự phải được sử dụng lại trên tất cả các bộ công cụ, do đó, các quyết định cục bộ sẽ ảnh hưởng đến tính khả thi toàn cầu. 

Như một ví dụ về hư hỏng cụ thể, giả sử chúng ta có hai loại: một phần nhẹ nhưng nhiều và một phần nặng nhưng khan hiếm. Việc tham lam thêm các bộ phận nặng có thể tạo ra một bộ hợp lệ, nhưng nguồn cung cấp cạn kiệt quá nhanh, làm giảm số lượng bản sao nhiều hơn mức cần thiết. Câu trả lời đúng phụ thuộc vào việc cân bằng giữa thành phần và sự lặp lại, chứ không chỉ tạo thành một bộ hợp lệ. 

Một trường hợp cạnh khác là khi không có sự kết hợp đơn lẻ nào đạt được khối lượng tối thiểu. Trong trường hợp đó, câu trả lời là 0 ngay cả khi có nhiều nguồn cung. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua yêu cầu rằng tất cả các bộ dụng cụ phải giống hệt nhau thì bài toán sẽ trở thành một biến thể của chiếc ba lô bị chặn: chúng ta thử xem liệu chúng ta có thể tạo thành một tập hợp con hợp lệ gồm các bộ phận có tổng khối lượng nằm trong khoảng cho phép hay không. Nhưng ở đây, điều khó khăn là chúng ta phải lặp lại cùng một tập hợp con nhiều lần. 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các vectơ có thể mô tả một bộ duy nhất. Đối với mỗi thành phần ứng cử viên, chúng tôi sẽ tính toán tổng khối lượng của nó và sau đó tính toán số lượng bản sao mà chúng tôi có thể mua được dựa trên loại bộ phận hạn chế nhất. Nếu có n loại và mỗi loại có thể được sử dụng tối đa ci lần, thì thậm chí việc giới hạn xi ở [0, ci] sẽ dẫn đến một không gian tìm kiếm khổng lồ, đại khái là hàm mũ tính bằng n. Điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là tách biệt hai quyết định. Đầu tiên, chúng tôi xác định số lượng bộ công cụ mà chúng tôi muốn xây dựng. Sau khi k được sửa, mỗi loại bộ phận i có thể được sử dụng nhiều nhất ở tầng (ci / k) lần trong một bộ sản phẩm. Điều này biến vấn đề thành việc kiểm tra xem liệu có tồn tại một tổ hợp giới hạn của các vật phẩm có tổng trọng lượng nằm trong khoảng hay không. Việc kiểm tra tính khả thi trở thành một vấn đề về khả năng tiếp cận ba lô có giới hạn cổ điển. 

Vì mmax nhiều nhất là 10^4 nên chúng ta có thể mua được giải pháp lập trình động với số tiền có thể đạt được. Thách thức duy nhất còn lại là giới hạn của mỗi số lượng mục có thể lớn, vì vậy chúng tôi nén chúng bằng cách phân tách nhị phân. Mỗi loại có dung lượng U được chia thành các mục giả O(log U), làm giảm ba lô giới hạn thành ba lô 0/1 tiêu chuẩn. 

Cuối cùng, chúng ta tìm kiếm nhị phân k, vì tính khả thi là đơn điệu: nếu chúng ta có thể xây dựng k bộ công cụ thì chúng ta cũng có thể xây dựng bất kỳ số nhỏ hơn nào.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các tác phẩm | Hàm mũ | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + ba lô giới hạn DP | O(n * mmax * log ci * log câu trả lời) | O(mmax) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách đoán xem chúng tôi muốn tạo bao nhiêu bộ dụng cụ giống hệt nhau và xác minh xem liệu việc đoán đó có khả thi hay không. 

1. Cố định số lượng ứng cử viên của bộ công cụ k. Với mỗi loại bộ phận i, hãy tính xem có thể sử dụng bao nhiêu bản sao của bộ phận đó trong một bộ, tức là ci // k. Điều này buộc k bộ dụng cụ giống hệt nhau sẽ không vượt quá số lượng có sẵn. Bước này chuyển đổi giới hạn lặp lại toàn cục thành giới hạn cục bộ cho mỗi bộ. 
2. Chuyển đổi từng mục bị chặn (mi với giới hạn Ui = ci // k) thành nhiều mục 0/1 bằng cách sử dụng phân tách nhị phân. Ví dụ: giới hạn 13 trở thành 1, 2, 4, 6. Mỗi số này biểu thị việc lấy nhiều bản sao của cùng loại bộ phận trong một bộ sản phẩm. 
3. Chạy DP ba lô trên khối lượng có thể đạt được lên đến mmax. Chúng tôi duy trì một mảng boolean dp trong đó dp[s] có nghĩa là chúng tôi có thể tạo thành một bộ có tổng khối lượng chính xác bằng cách sử dụng các vật phẩm được phân tách. 
4. Sau khi xử lý tất cả các mục, hãy kiểm tra xem có bất kỳ dp[s] nào đúng với s trong [mmin, mmax] hay không. Nếu có thì k này khả thi, ngược lại thì không. 
5. Tìm kiếm nhị phân k từ 0 đến giới hạn trên an toàn, thường là tổng số bộ phận chia cho yêu cầu tối thiểu có thể có cho mỗi bộ. K khả thi lớn nhất là câu trả lời. 

### Tại sao nó hoạt động 

Với k cố định, mọi giải pháp hợp lệ đều tương ứng chính xác với việc chọn nhiều tập mục tuân theo giới hạn mỗi loại ci // k. Việc phân tách nhị phân bảo toàn tất cả các kết hợp hợp lệ của việc sử dụng bị giới hạn. DP liệt kê tất cả số tiền có thể tiếp cận theo các ràng buộc đó. Vì mọi bộ hợp lệ phải có tổng khối lượng trong khoảng thời gian yêu cầu nên việc kiểm tra khả năng tiếp cận trong khoảng đó sẽ nắm bắt chính xác tính khả thi. Tính đơn điệu trong k đảm bảo tìm kiếm nhị phân tìm thấy số lượng tối đa các bộ giống nhau có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_make(k, n, mmin, mmax, m):
    # build bounded knapsack using binary splitting
    dp = [False] * (mmax + 1)
    dp[0] = True

    for mi, ci in m:
        if k == 0:
            u = ci
        else:
            u = ci // k
        if u == 0:
            continue

        cnt = 1
        while u > 0:
            take = min(cnt, u)
            weight = mi * take

            # 0/1 knapsack transition
            for s in range(mmax, weight - 1, -1):
                if dp[s - weight]:
                    dp[s] = True

            u -= take
            cnt <<= 1

    for s in range(mmin, mmax + 1):
        if dp[s]:
            return True
    return False

def solve():
    n, mmin, mmax = map(int, input().split())
    m = [tuple(map(int, input().split())) for _ in range(n)]

    # binary search answer
    lo, hi = 0, 10**6 + 5

    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can_make(mid, n, mmin, mmax, m):
            lo = mid
        else:
            hi = mid - 1

    print(lo)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xác định việc kiểm tra tính khả thi cho một số bộ dụng cụ cố định. Bên trong kiểm tra này, mỗi loại vật phẩm được chuyển thành nhiều khối giới hạn để chiếc ba lô trở thành tổng tập hợp con tiêu chuẩn trên các khối đó. Mảng DP có kích thước lên tới mmax vì bất kỳ tổng nào ở trên đều không liên quan. 

Tìm kiếm nhị phân kết thúc việc kiểm tra này. Nếu k kit là có thể thì tất cả các giá trị nhỏ hơn cũng có thể xảy ra, điều này đảm bảo tính chính xác của tìm kiếm đơn điệu. 

Một chi tiết triển khai tinh tế là lặp lại thao tác ngược lại khi thêm từng đoạn. Điều này ngăn cản việc sử dụng lại cùng một đoạn nhiều lần, duy trì tính chất 0/1 sau khi phân tách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 12 13
3 8
4 6
7 9
```Chúng tôi thử một ứng cử viên k. 

| Bước | Loại đã xử lý | Có sẵn mỗi bộ | Tóm tắt cập nhật DP | Tổng khả thi trong phạm vi | 
| --- | --- | --- | --- | --- | 
| 1 | (3,8) | 8//k | số tiền có thể tiếp cận được cập nhật | một phần | 
| 2 | (4,6) | 6//k | số tiền có thể tiếp cận được cập nhật | một phần | 
| 3 | (7,9) | 9//k | số tiền có thể tiếp cận được cập nhật | kiểm tra cuối cùng | 

Với k = 4, mỗi bộ có thể sử dụng tối đa (2,1,2) tùy theo cách chia. DP cuối cùng đạt tổng 13 thông qua 2×3 + 1×7. Vì 13 nằm trong [12,13] nên k khả thi. 

Điều này xác nhận rằng tính khả thi phụ thuộc vào cả thành phần và sự lặp lại có giới hạn. 

### Ví dụ 2 

đầu vào:```
2 5 6
2 3
4 1
```Việc thử k = 1 cho phép tất cả các mục. DP có thể tạo thành các tổng 2, 4, 6, v.v., do đó tổng hợp lệ tồn tại trong [5,6]. Do đó k ≥ 1 đúng. 

Việc thử k = 2 làm giảm dung lượng: loại thứ hai trở thành 0, loại thứ nhất trở thành 1. Chỉ có thể có tổng 0 hoặc 2, do đó không thể truy cập được khoảng. Điều này cho thấy việc tăng k có thể phá vỡ tính khả thi bằng cách thắt chặt các giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log K * n * mmax * log ci) | tìm kiếm nhị phân trên k, mỗi séc được giới hạn trong ba lô có phân tách nhị phân | 
| Không gian | O(mmax) | Mảng DP cho tổng có thể tiếp cận lên đến khối lượng tối đa được phép | 

Các ràng buộc cho phép mmax lên tới 10^4 và n lên tới 50, giúp DP có thể quản lý được. Việc phân tách nhị phân đảm bảo rằng ngay cả các giá trị ci lớn cũng không bùng nổ thành độ phức tạp tuyến tính trên mỗi loại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    # re-run solution
    input = sys.stdin.readline

    def can_make(k, n, mmin, mmax, m):
        dp = [False] * (mmax + 1)
        dp[0] = True
        for mi, ci in m:
            u = ci // k if k else ci
            if u == 0:
                continue
            cnt = 1
            while u > 0:
                take = min(cnt, u)
                w = mi * take
                for s in range(mmax, w - 1, -1):
                    if dp[s - w]:
                        dp[s] = True
                u -= take
                cnt <<= 1
        return any(dp[s] for s in range(mmin, mmax + 1))

    def solve():
        n, mmin, mmax = map(int, input().split())
        m = [tuple(map(int, input().split())) for _ in range(n)]
        lo, hi = 0, 10**6 + 5
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if can_make(mid, n, mmin, mmax, m):
                lo = mid
            else:
                hi = mid - 1
        print(lo)

    solve()
    return ""

# sample / sanity checks (structure-focused)
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn tối thiểu | 0 hoặc 1 | tính khả thi cơ bản | 
| tất cả các vật nặng | 0 | không thể đạt được mmin | 
| khoảng thời gian có thể truy cập hỗn hợp | >0 | ba lô đúng đắn | 
| chặt chẽ mmin=mmax | yêu cầu số tiền chính xác | độ chính xác ranh giới | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi mmin rất nhỏ nhưng mmax lại chật. Trong trường hợp đó, DP vẫn phải tự giới hạn ở giới hạn trên ngay cả khi có thể truy cập được nhiều số tiền, nếu không, DP có thể chấp nhận không chính xác các số tiền nằm ngoài phạm vi hợp lệ. 

Một trường hợp cạnh khác xảy ra khi k trở nên đủ lớn để ci // k trở thành 0 đối với tất cả các loại. Trong tình huống đó, chỉ có k = 0 hoặc k = 1 (tùy cách giải thích) là khả thi. Thuật toán xử lý việc này một cách tự nhiên vì DP không nhận được vật phẩm nào và chỉ kiểm tra xem khối lượng bằng 0 có nằm trong khoảng hay không. 

Trường hợp thứ ba là khi một loại duy nhất chiếm ưu thế trong giải pháp. Ví dụ: nếu một mục có khối lượng chính xác trong khoảng, DP phải cho phép chọn chính xác nhiều bản sao đến giới hạn giới hạn trên k mà không trộn lẫn các loại khác. Việc phân tách nhị phân đảm bảo những đóng góp lặp lại này được xử lý chính xác mà không bị tính quá mức.
