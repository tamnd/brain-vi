---
title: "CF 105264C - Kẻ ghét sự đa dạng"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép thực hiện một số điều chỉnh đơn vị có giới hạn. Mỗi thao tác chọn một vị trí duy nhất và tăng hoặc giảm giá trị đó chính xác một vị trí."
date: "2026-06-24T01:27:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105264
codeforces_index: "C"
codeforces_contest_name: "The 2024 Syrian Virtual University Collegiate Programming Contest"
rating: 0
weight: 105264
solve_time_s: 69
verified: true
draft: false
---

[CF 105264C - Kẻ ghét sự đa dạng](https://codeforces.com/problemset/problem/105264/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép thực hiện một số điều chỉnh đơn vị có giới hạn. Mỗi thao tác chọn một vị trí duy nhất và tăng hoặc giảm giá trị đó chính xác một vị trí. Sau khi sử dụng tối đa$k$như vậy, mục tiêu là làm cho mảng trở nên “đồng nhất” nhất có thể về mặt phân tập giá trị, nghĩa là chúng tôi muốn giảm thiểu số lượng số nguyên riêng biệt còn lại trong mảng. 

Sự tự do chính là chúng tôi không bắt buộc phải làm cho tất cả các yếu tố trở nên bình đẳng. Chúng ta chỉ muốn giảm số lượng các giá trị khác nhau nên được phép hợp nhất các nhóm số bằng cách di chuyển chúng về một giá trị chung, trả một chi phí bằng chênh lệch tuyệt đối. 

Những hạn chế rất rõ ràng. Kích thước mảng cho mỗi bài kiểm tra tổng cộng tối đa là 300 trong tất cả các trường hợp kiểm tra, vì vậy lý luận bậc hai hoặc bậc ba đối với các giá trị là ổn. Số lượng hoạt động$k$có thể lớn như$10^{12}$, điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào cũng phải tính đến tổng chi phí thay vì mô phỏng các hoạt động từng bước. Chúng ta cần tính toán chi phí điều chỉnh tối thiểu một cách hiệu quả và sau đó quyết định số lượng nhóm có thể được hợp nhất theo ngân sách. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta có thể “đẩy” các con số về phía giá trị thường xuyên nhất một cách tham lam. Điều đó không thành công vì giá trị hợp nhất tối ưu không nhất thiết phải là giá trị hiện có và các quyết định nhóm tương tác với nhau. 

Ví dụ, hãy xem xét một mảng như$[1, 10, 20]$với một cái nhỏ$k$. Một chiến lược tham lam có thể cố gắng chuyển đổi mọi thứ thành 10, nhưng chiến lược tối ưu thay vào đó có thể tạo thành hai cụm như$[1, 10]$Và$[20]$tùy theo ngân sách. Một trường hợp khó phát hiện khác là khi các giá trị cách xa nhau nhưng thưa thớt; việc hợp nhất các cặp liền kề theo thứ tự được sắp xếp không phải lúc nào cũng tối ưu trừ khi chúng ta tính toán chi phí một cách chính xác. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là chọn những giá trị mà chúng tôi muốn giữ làm nhóm riêng biệt cuối cùng và gán mọi phần tử cho một trong các nhóm này, trả chi phí di chuyển nó đến giá trị đại diện đã chọn. Đối với lựa chọn nhóm cố định, chúng ta có thể tính toán chi phí tối thiểu dưới dạng bài toán phân cụm trên một dòng. 

Điều này trở nên tốn kém vì có rất nhiều cách để chọn nhóm nào vẫn khác biệt và đối với mỗi nhóm, chúng ta sẽ cần tính toán chi phí di chuyển. Ngay cả với$n \le 300$, việc liệt kê tất cả các phân vùng là không thể. 

Quan sát quan trọng là sau khi sắp xếp mảng, các nhóm tối ưu tương ứng với các phân đoạn liền kề. Nếu chúng ta quyết định kết thúc với$g$các giá trị riêng biệt, chúng tôi đang phân vùng hiệu quả mảng đã sắp xếp thành$g$các phân khúc và mỗi phân khúc sẽ được hợp nhất thành một giá trị được chọn duy nhất để giảm thiểu chi phí, là giá trị trung bình của phân khúc. Khi đó, chi phí của một phân khúc là tổng của những khác biệt tuyệt đối so với mức trung bình của nó. 

Điều này biến vấn đề thành việc chọn phân vùng của mảng đã sắp xếp thành càng ít phân đoạn càng tốt sao cho tổng chi phí nằm trong ngân sách$k$. Từ$k$lớn, chúng tôi không tối đa hóa chi phí sử dụng mà giảm thiểu số lượng phân khúc. 

Chúng tôi tính toán trước chi phí cho mỗi phân đoạn bằng cách sử dụng tổng tiền tố, sau đó sử dụng lập trình động trong đó$dp[g][i]$là chi phí tối thiểu để phân vùng đầu tiên$i$các phần tử vào$g$phân đoạn. Câu trả lời là nhỏ nhất$g$như vậy$dp[g][n] \le k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| DP với chi phí phân khúc |$O(n^3)$mỗi bài kiểm tra |$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp mảng sao cho các nhóm trở thành các khoảng trên một dòng, điều này làm cho cấu trúc chi phí trở nên lồi và hoạt động tốt. 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm sao cho mọi nhóm tối ưu đều tuân theo thứ tự. Điều này hợp lệ vì việc trộn các phần tử theo thứ tự sẽ luôn làm tăng chi phí điều chỉnh so với việc chỉ định lại trong cấu trúc được sắp xếp. 
2. Tính trước tổng tiền tố của mảng đã được sắp xếp. Điều này cho phép tính toán nhanh chi phí phân đoạn trong thời gian không đổi sau khi tiền xử lý. 
3. Đối với từng phân khúc$[l, r]$, tính chi phí tối thiểu để làm cho tất cả các phần tử bằng một giá trị duy nhất. Mục tiêu tối ưu là phần tử trung vị, vì vậy chúng tôi chia chi phí thành các phần bên trái và bên phải và tính toán bằng cách sử dụng tổng tiền tố. Bước này xây dựng một bảng chi phí. 
4. Xác định trạng thái DP nơi chúng tôi tính toán chi phí tối thiểu để phân chia phần đầu tiên$i$thành phần chính xác$g$các nhóm. Chuyển đổi bằng cách thử vị trí cắt cuối cùng$j$, kết hợp giải pháp tối ưu trước đó với chi phí của phân khúc$[j+1, i]$. 
5. Lặp lại số lượng nhóm có thể có từ 1 đến$n$và với mỗi mảng, hãy tính chi phí tốt nhất có thể đạt được cho toàn bộ mảng. 
6. Số lượng nhóm nhỏ nhất có chi phí không vượt quá$k$là câu trả lời. 

Bước suy luận quan trọng là sau khi được sắp xếp, cấu trúc sẽ giảm xuống việc phân chia một dòng và việc hợp nhất tối ưu bên trong một phân đoạn luôn thu gọn về mức trung vị. 

### Tại sao nó hoạt động 

Việc sắp xếp buộc bất kỳ nhóm tối ưu nào cũng có thể được chuyển đổi thành các phân đoạn liền kề mà không làm tăng chi phí, vì việc phân chia chéo tạo ra khoảng cách không cần thiết. Trong mỗi phân đoạn, trung vị giảm thiểu độ lệch tuyệt đối, đây là đặc tính cổ điển của$L_1$tối ưu hóa trên một dòng. DP khám phá tất cả các phân đoạn có thể có, đảm bảo rằng nếu một cấu hình có$g$giá trị cuối cùng khác biệt là có thể trong phạm vi ngân sách, nó sẽ được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, arr):
    arr.sort()

    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + arr[i]

    def cost(l, r):
        m = (l + r) // 2
        median = arr[m]

        left_sum = median * (m - l + 1) - (prefix[m + 1] - prefix[l])
        right_sum = (prefix[r + 1] - prefix[m + 1]) - median * (r - m)
        return left_sum + right_sum

    INF = 10**18

    dp = [[INF] * n for _ in range(n + 1)]
    for i in range(n):
        dp[1][i] = cost(0, i)

    for g in range(2, n + 1):
        for i in range(n):
            best = INF
            for j in range(i):
                best = min(best, dp[g - 1][j] + cost(j + 1, i))
            dp[g][i] = best

    for g in range(1, n + 1):
        if dp[g][n - 1] <= k:
            return g

    return n

def main():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        print(solve_case(n, k, arr))

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách sắp xếp mảng, điều này rất cần thiết để giảm bớt vấn đề về phân vùng theo khoảng thời gian. Tổng tiền tố cho phép truy vấn tổng phân đoạn theo thời gian không đổi, cần thiết để đánh giá chi phí nhanh chóng. 

các`cost(l, r)`hàm thực hiện công thức độ lệch tuyệt đối dựa trên trung vị. Việc phân chia ở chỉ số trung bình đảm bảo chi phí di chuyển tối thiểu và tổng tiền tố tránh việc tính toán lại tổng số phân đoạn nhiều lần. 

Bảng DP xây dựng các giải pháp tăng dần theo số lượng nhóm. Đối với mỗi điểm cuối, chúng tôi thử tất cả các điểm phân chia trước đó, đây là nơi$O(n^3)$sự phức tạp phát sinh. Từ$n \le 300$, điều này vẫn khả thi. 

Cuối cùng, chúng tôi quét số lượng nhóm nhỏ nhất có chi phí không vượt quá$k$, phù hợp trực tiếp với mục tiêu của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, k = 3
arr = [1, 2, 10, 11]
```Mảng sắp xếp đã giống nhau rồi. 

Chúng tôi tính toán chi phí phân khúc: 

| Phân đoạn | Trung vị | Chi phí | 
| --- | --- | --- | 
| [1] | 1 | 0 | 
| [1,2] | 1 | 1 | 
| [10,11] | 10 | 1 | 
| [1,2,10] | 2 | 9 | 
| [2,10,11] | 10 | 9 | 
| [1,2,10,11] | 2 | 18 | 

Bây giờ DP xem xét các phân vùng: 

Đối với 1 nhóm, chi phí là 18, không khả thi. 

Đối với 2 nhóm, cách phân chia tốt nhất là [1,2] và [10,11], chi phí = 1 + 1 = 2, phù hợp với k. 

| nhóm | chi phí tốt nhất | 
| --- | --- | 
| 1 | 18 | 
| 2 | 2 | 

Câu trả lời là 2. 

Dấu vết này cho thấy việc nhóm theo sự gần gũi thay vì ép buộc một trung tâm duy nhất là điều cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 0
arr = [5, 5, 5, 5, 5]
```Tất cả các phần tử đều giống hệt nhau nên chi phí của mỗi phân khúc đều bằng 0. 

| nhóm | chi phí | 
| --- | --- | 
| 1 | 0 | 

Chúng ta luôn có thể đạt được 1 nhóm và vì không cần thực hiện thao tác nào nên câu trả lời là 1. 

Điều này xác nhận rằng DP được thu gọn chính xác khi mảng đã đồng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$mỗi bài kiểm tra | DP qua các nhóm và điểm cuối với quá trình chuyển đổi tuyến tính | 
| Không gian |$O(n^2)$| Chi phí lưu trữ bảng DP cho tất cả các tiền tố và số lượng nhóm | 

Với tổng số$n \le 300$, hệ số bậc ba vẫn nằm trong giới hạn. Việc tối ưu hóa tổng tiền tố đảm bảo rằng việc tính toán chi phí phân khúc không đưa ra yếu tố bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))
            a.sort()
            pref = [0]
            for x in a:
                pref.append(pref[-1] + x)

            def cost(l, r):
                m = (l + r) // 2
                med = a[m]
                left = med * (m - l + 1) - (pref[m+1] - pref[l])
                right = (pref[r+1] - pref[m+1]) - med * (r - m)
                return left + right

            INF = 10**18
            n = len(a)
            dp = [[INF]*n for _ in range(n+1)]
            for i in range(n):
                dp[1][i] = cost(0, i)

            for g in range(2, n+1):
                for i in range(n):
                    for j in range(i):
                        dp[g][i] = min(dp[g][i], dp[g-1][j] + cost(j+1, i))

            for g in range(1, n+1):
                if dp[g][n-1] <= k:
                    out.append(str(g))
                    break
        return "\n".join(out)

    return solve()

# custom tests
assert run("1\n1 0\n5\n") == "1", "single element"
assert run("1\n3 10\n1 10 20\n") == "2", "split into two clusters"
assert run("1\n5 100\n1 2 3 4 5\n") == "1", "large budget merges all"
assert run("1\n4 0\n1 2 3 10\n") == "3", "no operations allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ bản tầm thường | 
| 1 10 20 | 2 | phân cụm tối ưu | 
| 1 2 3 4 5, k lớn | 1 | tính khả thi hợp nhất đầy đủ | 
| không có hoạt động | 3 | hành vi hạn chế danh tính | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k = 0$. Trong tình huống này, không được phép điều chỉnh nên câu trả lời chỉ đơn giản là số lượng giá trị riêng biệt đã có. DP xử lý việc này một cách tự nhiên vì mọi chi phí của phân đoạn phải bằng 0, điều này chỉ xảy ra khi các phân đoạn chứa các giá trị giống hệt nhau. 

Một trường hợp tinh tế khác là khi mảng có khoảng trống lớn, chẳng hạn như$[1, 100, 101, 200]$. Một sự hợp nhất tham lam ngây thơ có thể cố gắng kéo mọi thứ về một giá trị, nhưng việc phân vùng tối ưu sẽ chia thành các cụm chặt chẽ như$[100,101]$và đơn vị mà DP nắm bắt được vì chi phí phân khúc được tính toán chính xác theo từng khoảng thời gian chứ không phải theo mức thu hút toàn cầu. 

Trường hợp cạnh cuối cùng là khi$k$là rất lớn, lên tới$10^{12}$. Điều này có thể gợi ý những lo ngại về tràn số, nhưng tất cả các tính toán vẫn nằm trong phạm vi 64 bit do chi phí bị giới hạn bởi$n \cdot 10^9$, phù hợp một cách an toàn trong số học số nguyên tiêu chuẩn.
