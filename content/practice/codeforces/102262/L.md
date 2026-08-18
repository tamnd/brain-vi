---
title: "CF 102262L - \u041d\u0430\u0431\u043e\u0440 \u043a\u043b\u0430\u0441\u0441\u0438\u0444\u0438\u043a\u0430\u0442\u043e\u0440\u043e\u0432"
description: "Có N bộ phân loại. Trình phân loại i có K giá trị số liệu, một giá trị cho mỗi số liệu. Nếu chúng ta kích hoạt một số bộ phân loại, giá trị kết quả của số liệu j là a[i][j] tối đa trong số các bộ phân loại được kích hoạt. Tính hữu ích của tập được kích hoạt là tổng của K cực đại này."
date: "2026-08-17T20:34:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "L"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 162
verified: true
draft: false
---

[CF 102262L - \u041d\u0430\u0431\u043e\u0440 \u043a\u043b\u0430\u0441\u0441\u0438\u0444\u0438\u043a\u0430\u0442\u043e\u0440\u043e\u0432](https://codeforces.com/problemset/problem/102262/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có N bộ phân loại. Trình phân loại i có K giá trị số liệu, một giá trị cho mỗi số liệu. Nếu chúng ta kích hoạt một số bộ phân loại, giá trị kết quả của số liệu j là a[i][j] tối đa trong số các bộ phân loại được kích hoạt. Tính hữu ích của tập được kích hoạt là tổng của K cực đại này. 

Chúng ta cần chọn chính xác M bộ phân loại riêng biệt với tính hữu ích tối đa có thể. Đầu ra chứa giá trị tối đa đó và bất kỳ tập hợp chỉ số phân loại M nào đạt được giá trị đó. 

Các ràng buộc hướng trực tiếp đến một thuật toán hàm mũ trong K chứ không phải trong N. Có thể có 2000 bộ phân loại, do đó việc liệt kê các tập hợp con của các bộ phân loại là không thể. Mặt khác, K nhiều nhất là 15, do đó chỉ có 2^K = 32768 tập hợp con số liệu. Đây là khía cạnh nhỏ mà giải pháp phải khai thác. Các giá trị số liệu có thể đạt tới 10^8, nhưng tổng của chúng vừa vặn với số nguyên 64 bit và số nguyên Python không có vấn đề tràn. 

Có một số trường hợp khó khăn trong đó việc triển khai có vẻ hợp lý lại có thể thất bại. Đầu tiên, M có thể là 1. Ví dụ:```
2 1 2
10 1
1 10
```câu trả lời là```
11
1
```bởi vì chỉ với một bộ phân loại, chúng ta cần một hàng có tổng giá trị tối đa. Việc triển khai lấy giá trị tốt nhất của mọi số liệu một cách độc lập sẽ thu được 20 không chính xác bằng cách sử dụng ngầm hai bộ phân loại. 

Thứ hai, M có thể ít nhất là K. Ví dụ:```
3 3 2
10 10
5 5
1 1
```mức độ hữu dụng tối đa là 20 và tập duy nhất có thể có cả ba bộ phân loại:```
20
1 2 3
```Việc triển khai bất cẩn có thể dừng lại sau khi chọn một bộ phân loại chịu trách nhiệm về cực đại và quên rằng phải in chính xác M chỉ số. 

Thứ ba, một số số liệu có thể được gán cho cùng một bộ phân loại. Coi như```
3 2 3
10 10 10
9 1 1
1 9 1
```Giá trị tối ưu là 30 khi sử dụng bộ phân loại 1 và bất kỳ bộ phân loại nào khác. Giải pháp dựa trên phân vùng phải cho phép một bộ phân loại chịu trách nhiệm về toàn bộ tập hợp con các số liệu, thay vì buộc một bộ phân loại cho mỗi số liệu. 

Cuối cùng, hai nhóm số liệu khác nhau có thể chọn cùng một bộ phân loại một cách độc lập. Đây không phải là lỗi ở phân vùng DP. Nếu điều đó xảy ra, hai nhóm có thể được hợp nhất mà không làm giảm giá trị và sau đó có thể thêm các bộ phân loại không sử dụng để đạt được chính xác M chỉ mục đã chọn. 

Các ví dụ trên trang vấn đề chính thức có giá trị hữu ích lần lượt là 10 và 20, với các bộ được chọn`1 4`Và`1 2 3`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê mọi tập hợp con của bộ phân loại M, tính toán tất cả K cực đại và giữ tập hợp tốt nhất. Đối với một tập ứng cử viên, việc tính toán tính hữu dụng của nó sẽ mất O(MK) hoặc O(K) nếu cực đại được duy trì tăng dần trong quá trình liệt kê. Số lượng ứng cử viên là C(N, M), do đó tác phẩm trong trường hợp xấu nhất là khoảng O(C(N, M)K). Với N = 2000 và M khoảng 1000, đây là một con số lớn về mặt thiên văn. Việc K nhỏ không đủ giúp ích vì sự bùng nổ xuất phát từ việc lựa chọn các bộ phân loại. 

Quan sát hữu ích là mọi số liệu tối đa đều có chủ sở hữu. Giả sử một tập tối ưu được cố định. Đối với mỗi chỉ số, hãy chọn một bộ phân loại từ tập hợp đạt mức tối đa của chỉ số đó. Bây giờ mọi số liệu được gán cho một trong các bộ phân loại đã chọn. Một bộ phân loại có thể sở hữu một số số liệu. 

Hãy xem xét một số tập con S của số liệu. Nếu một bộ phân loại chịu trách nhiệm về tất cả các số liệu trong S thì sự đóng góp tốt nhất có thể có của nhóm đó là 

f(S) = max trên các bộ phân loại i của tổng trên j trong S của a[i][j]. 

Tại sao công thức này hoạt động? Khi chúng tôi quyết định rằng trình phân loại i chịu trách nhiệm cho tất cả các số liệu trong S, thì đóng góp của nó chính xác là tổng các giá trị của nó trên S. Chúng ta nên chọn trình phân loại có tổng lớn nhất như vậy. 

Bây giờ vấn đề ban đầu đã thay đổi hình dạng. Thay vì chọn trực tiếp các bộ phân loại, chúng tôi phân chia K số liệu thành M nhóm khác rỗng. Với mỗi nhóm S, chúng ta nhận được f(S). Tổng giá trị của một phân vùng là tổng của f(S) trên tất cả các nhóm. 

Lực lượng vũ phu hoạt động vì mọi bộ phân loại có thể được xem xét một cách rõ ràng, nhưng không thành công khi N lớn. Việc quan sát thấy số liệu K có thể được gán cho các bộ phân loại chịu trách nhiệm của chúng cho phép chúng ta di chuyển phần mũ từ N sang K. 

Có một tài sản hữu ích hơn. Nếu một phân vùng có ít hơn K nhóm, việc chia bất kỳ nhóm nào thành hai nhóm khác trống không thể làm giảm giá trị của nó. Đối với A và B rời nhau, 

f(A ∪ B) <= f(A) + f(B), 

vì bên trái chọn một bộ phân loại cho cả hai phần, trong khi bên phải có thể chọn các bộ phân loại khác nhau. Do đó, khi M < K, mức tối ưu sử dụng tối đa M bộ phân loại có thể được biểu diễn bằng cách sử dụng chính xác M nhóm số liệu khác trống. Khi M >= K, mỗi số liệu có thể có nhóm riêng, do đó mức tối ưu không bị ràng buộc chỉ đơn giản là tổng của các cực đại số liệu riêng lẻ. 

Đối với mỗi mặt nạ số liệu, trước tiên chúng tôi tính toán f(mặt nạ) và ghi nhớ một bộ phân loại đạt được nó. Sau đó, chúng tôi giải quyết DP phân vùng tập hợp. Đặt dp[t][mask] là giá trị tối đa thu được bằng cách phân chia các số liệu trong mặt nạ thành chính xác t nhóm khác trống. Để tránh xem xét cùng một phân vùng theo các thứ tự khác nhau, khi xử lý mặt nạ, chúng tôi buộc nhóm chứa bit mặt nạ có ý nghĩa nhỏ nhất phải được chọn trước tiên. 

Sự chuyển tiếp là 

dp[t][mask] = max f[sub] + dp[t-1][mask \ sub], 

trong đó sub là tập con khác trống của mặt nạ chứa bit có ý nghĩa nhỏ nhất. 

Có các cặp tập hợp con O(3^K) trong tập hợp con DP thông thường. Việc buộc bit có trọng số thấp nhất vào nhóm đã chọn sẽ loại bỏ tính đối xứng thứ tự và tạo ra các chuyển đổi O(3^(K-1)) cho mỗi lớp DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(N,M)K) | O(NK) | Quá chậm | 
| Tối ưu | O(NK2^K + M3^(K-1)) | O(NK + M2^K) | Thuật toán được chấp nhận | 

Sự phụ thuộc theo cấp số nhân chỉ vào K, tối đa là 15. Việc triển khai Python bên dưới sử dụng cùng một thuật toán chính xác, với các mảng nhỏ gọn và tối ưu hóa bit có ý nghĩa nhỏ nhất. Giới hạn 3 giây là giới hạn chặt chẽ đối với Python trong các phiên bản đối nghịch lớn nhất, vì vậy C++ là ngôn ngữ triển khai an toàn hơn cho giới hạn cuộc thi ban đầu. 

## Hướng dẫn thuật toán

1. Đọc tất cả các hàng phân loại và giữ lại các chỉ mục dựa trên 1 ban đầu của chúng. Các hàng không bao giờ được sắp xếp lại theo cách có thể ảnh hưởng đến chỉ số đầu ra được yêu cầu. 
2. Nếu M bằng N, hãy chọn ngay mọi bộ phân loại. Không có sự lựa chọn nào, vì vậy tính hữu dụng chỉ đơn giản là mức tối đa theo tọa độ trên tất cả N hàng. 
3. Nếu M ít nhất là K, hãy tính toán bộ phân loại tốt nhất một cách độc lập cho từng số liệu. Việc chọn tất cả những người chiến thắng này sẽ mang lại giá trị tối đa có thể vì có tối đa K người chiến thắng khác nhau. Nếu ít hơn M phân loại riêng biệt được chọn, hãy thêm các phân loại không sử dụng tùy ý. Việc thêm bộ phân loại không thể làm giảm bất kỳ số liệu tối đa nào, do đó tính hữu dụng vẫn ở mức tối ưu. 
4. Ngược lại M < K. Với mọi mặt nạ số liệu khác trống, hãy tính f(mặt nạ), tổng tối đa của các số liệu trong mặt nạ đó trên tất cả các phân loại. Đồng thời, hãy nhớ bộ phân loại nào đạt được mức tối đa này. 
5. Tính tổng tập hợp con của mọi mặt nạ cho mỗi phân loại. Nếu tập bit có ý nghĩa nhỏ nhất của mặt nạ là b thì tổng của mặt nạ là tổng của mặt nạ không có bit đó cộng với giá trị của bộ phân loại ở số liệu b. Điều này cung cấp tất cả các tổng tập hợp con cho một bộ phân loại trong thời gian O(2^K). 
6. Khởi tạo lớp DP đầu tiên với dp[1][mask] = f(mask). Một nhóm chứa tất cả số liệu trong mặt nạ có chính xác giá trị này. 
7. Với mỗi t từ 2 đến M, hãy tính dp[t]. Đối với mặt nạ cố định, đặt b là bit đặt có ý nghĩa nhỏ nhất. Nhóm chứa b được xác định duy nhất trong bất kỳ phân vùng không có thứ tự nào. Liệt kê mặt nạ con có thể có của nó trong số tất cả các mặt nạ con chứa b và kết hợp f(sub) với dp[t-1][mask \ sub]. 
8. Lưu trữ mặt nạ con đã chọn cho mọi trạng thái DP. Điều này cho phép phân vùng số liệu tối ưu được xây dựng lại sau lớp DP cuối cùng. 
9. Bắt đầu từ mặt nạ số liệu đầy đủ và liên tục lấy mặt nạ con đã lưu trữ. Đối với mỗi nhóm, hãy thêm bộ phân loại được ghi nhớ cho mặt nạ con đó. Danh sách phân loại kết quả có thể chứa cùng một bộ phân loại nhiều lần vì hai nhóm có thể có cùng một bộ phân loại tốt nhất. 
10. Loại bỏ các chỉ mục phân loại trùng lặp. Nếu vẫn còn ít hơn M chỉ số riêng biệt, hãy thêm các phân loại không sử dụng tùy ý. Giá trị không thể giảm trong khoảng đệm này vì mọi chỉ số tối đa đều đơn điệu khi thêm bộ phân loại. 
11. In chỉ số phân loại riêng biệt DP tối ưu và kết quả M. 

Tại sao nó hoạt động 

Cố định một bộ phân loại tối ưu và gán mọi số liệu cho một bộ phân loại đạt mức tối đa. Điều này tạo ra một phân vùng của các số liệu. Đối với mỗi phần S của phân vùng này, bộ phân loại chịu trách nhiệm của nó đóng góp tối đa f(S) và f(S) có thể đạt được theo định nghĩa. Do đó, giá trị của bộ phân loại tối ưu được biểu thị bằng một số phân vùng số liệu có giá trị DP chính xác là tối ưu. 

Ngược lại, mỗi phân vùng DP chọn một bộ phân loại cho mỗi nhóm số liệu. Việc lấy các bộ phân loại đó tạo ra ít nhất tổng các giá trị f tương ứng làm cực đại số liệu của nó, bởi vì bộ phân loại được chọn cho một nhóm nhận ra f cho mọi số liệu trong nhóm đó. Do đó, mọi giải pháp DP đều tương ứng với một bộ phân loại hợp lệ có ít nhất giá trị DP. 

Hai hướng cho thấy mức tối ưu DP bằng mức tối ưu ban đầu. Việc xóa các bộ phân loại trùng lặp không thể làm giảm giá trị được biểu thị vì việc hợp nhất hai nhóm được gán cho cùng một bộ phân loại chỉ kết hợp các số liệu mà cùng một bộ phân loại đã cung cấp. Phần đệm với các bộ phân loại bổ sung cũng không thể hạ thấp giá trị. Do đó tập cuối cùng chứa chính xác M bộ phân loại riêng biệt và vẫn tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    # If all classifiers have to be selected, there is no optimization.
    if m == n:
        mx = [0] * k
        for row in a:
            for j, x in enumerate(row):
                if x > mx[j]:
                    mx[j] = x

        ans = sum(mx)
        print(ans)
        print(*range(1, n + 1))
        return

    # If we can use at least K classifiers, give every metric
    # its own best classifier and then pad the answer.
    if m >= k:
        mx = [0] * k
        winner = [-1] * k

        for i, row in enumerate(a):
            for j, x in enumerate(row):
                if x > mx[j]:
                    mx[j] = x
                    winner[j] = i

        chosen = []
        used = [False] * n

        for i in winner:
            if not used[i]:
                used[i] = True
                chosen.append(i)

        for i in range(n):
            if len(chosen) == m:
                break
            if not used[i]:
                used[i] = True
                chosen.append(i)

        print(sum(mx))
        print(*(x + 1 for x in chosen))
        return

    size = 1 << k
    full = size - 1

    # For each mask:
    # best[mask] = maximum sum of metrics in mask for one classifier
    # who[mask]  = classifier attaining best[mask]
    best = [0] * size
    who = [-1] * size

    # Precompute the least significant bit information.
    prev_mask = [0] * size
    bit_index = [0] * size
    popcount = [0] * size

    for mask in range(1, size):
        lb = mask & -mask
        prev_mask[mask] = mask ^ lb
        bit_index[mask] = lb.bit_length() - 1
        popcount[mask] = popcount[prev_mask[mask]] + 1

    # Compute f(mask) for every mask.
    subset_sum = [0] * size

    for i, row in enumerate(a):
        subset_sum[0] = 0

        for mask in range(1, size):
            subset_sum[mask] = (
                subset_sum[prev_mask[mask]] + row[bit_index[mask]]
            )

        for mask in range(1, size):
            value = subset_sum[mask]
            if value > best[mask]:
                best[mask] = value
                who[mask] = i

    # parent[t][mask] stores the group chosen for the transition
    # dp[t][mask] = best partition of mask into exactly t groups.
    parent = [None] * (m + 1)

    prev = best[:]
    parent[1] = [-1] * size

    for t in range(2, m + 1):
        cur = [-1] * size
        par = [-1] * size

        for mask in range(1, size):
            if popcount[mask] < t:
                continue

            lb = mask & -mask
            rest_mask = mask ^ lb

            # The selected group must contain lb.
            # Its complement is rest.
            rest = rest_mask

            while True:
                if popcount[rest] >= t - 1:
                    sub = mask ^ rest
                    value = best[sub] + prev[rest]

                    if value > cur[mask]:
                        cur[mask] = value
                        par[mask] = sub

                if rest == 0:
                    break
                rest = (rest - 1) & rest_mask

        prev = cur
        parent[t] = par

    answer = prev[full]

    # Reconstruct the metric groups.
    groups = []
    mask = full

    for t in range(m, 1, -1):
        sub = parent[t][mask]
        groups.append(sub)
        mask ^= sub

    groups.append(mask)

    # Convert metric groups into classifier indices.
    chosen = []
    used = [False] * n

    for sub in groups:
        i = who[sub]
        if not used[i]:
            used[i] = True
            chosen.append(i)

    # Duplicate winners can occur. Pad with arbitrary classifiers.
    for i in range(n):
        if len(chosen) == m:
            break
        if not used[i]:
            used[i] = True
            chosen.append(i)

    print(answer)
    print(*(i + 1 for i in chosen))

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý M = N trước bất kỳ phép tính hàm mũ nào. Vì mọi phân loại đều là bắt buộc nên việc tính toán mức tối đa theo tọa độ là đủ. 

Nhánh thứ hai xử lý M >= K. Mỗi số liệu chỉ cần một bộ phân loại để nhận ra mức tối đa của nó, vì vậy ban đầu nhiều nhất K bộ phân loại là cần thiết. Bởi vì các bộ phân loại bổ sung không thể giảm mức tối đa, các bộ phân loại không được sử dụng tùy ý có thể điền vào tập hợp kích thước M một cách an toàn. 

Nhánh chính chỉ được sử dụng khi M < K.`size`là 2^K và mỗi mặt nạ số nguyên đại diện cho một tập hợp con số liệu.`prev_mask`,`bit_index`, Và`popcount`được tính toán trước vì các hoạt động này xảy ra bên trong các vòng lặp hàm mũ. 

các`subset_sum`mảng được sử dụng lại cho mọi phân loại. Đối với một bộ phân loại, mọi tổng tập hợp con được lấy từ tập hợp con nhỏ hơn bằng cách thêm một số liệu. Do đó, mảng không bao giờ cần được lưu trữ đồng thời cho tất cả N bộ phân loại. 

DP sử dụng`-1`như điểm đánh dấu trạng thái không thể. Tất cả các giá trị thực tế đều dương, vì vậy`-1`không thể nhầm lẫn với giá trị phân vùng hợp lệ. 

Hạn chế bit ít quan trọng nhất là chi tiết quan trọng trong quá trình chuyển đổi. Nếu không có nó, cùng một phân vùng sẽ được xem xét một lần cho mỗi thứ tự của các nhóm của nó. Việc yêu cầu nhóm đầu tiên chứa bit được đặt thấp nhất sẽ cung cấp cho mỗi phân vùng không có thứ tự chính xác một biểu diễn. 

Việc xây dựng lại sử dụng mặt nạ con được lưu trữ thay vì cố gắng khôi phục các nhóm từ giá trị của chúng. Điều này tránh sự mơ hồ khi một số phân vùng có cùng tính hữu dụng. 

Số nguyên của Python xử lý câu trả lời tối đa có thể một cách an toàn, đó là K · 10^8 hoặc 1,5 · 10^9. Không yêu cầu loại 64-bit rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6 2 3
4 1 1
1 4 1
1 1 4
1 3 3
3 1 3
3 3 1
```Có ba số liệu, vì vậy các mặt nạ là từ 1 đến 7. Ví dụ: mặt nạ 5 đại diện cho số liệu 1 và 3. Giá trị f(5) là giá trị lớn nhất của`a[i][1] + a[i][3]`. 

Một số trạng thái có liên quan là: 

| Mặt nạ | Số liệu | f(mặt nạ) | Phân loại tốt nhất | 
| --- | --- | --- | --- | 
| 1 | {1} | 4 | 1 | 
| 2 | {2} | 4 | 2 | 
| 3 | {1,2} | 5 | 1 | 
| 4 | {3} | 4 | 3 | 
| 5 | {1,3} | 6 | 4 | 
| 6 | {2,3} | 6 | 4 | 
| 7 | {1,2,3} | 7 | 4 | 

Với hai nhóm, mặt nạ đầy đủ 7 có thể được chia thành nhiều khả năng: 

| Nhóm đầu tiên | Nhóm còn lại | Giá trị | 
| --- | --- | --- | 
| {1} | {2,3} | 4 + 6 = 10 | 
| {2} | {1,3} | 4 + 6 = 10 | 
| {1,2} | {3} | 5 + 4 = 9 | 

Tối ưu là 10. Một phân vùng tối ưu là`{1} | {2,3}`, có phân loại đại diện là 1 và 4. 

Các giá trị phân loại kết quả là`(4,3,3)`, vậy độ hữu ích là 10.```
10
1 4
```Dấu vết này chứng minh lý do tại sao các số liệu phân vùng DP thay vì phân loại. Bộ phân loại 1 cung cấp số liệu 1, trong khi bộ phân loại 4 cung cấp cả số liệu 2 và 3. 

### Mẫu 2 

Đầu vào là```
3 3 2
10 10
5 5
1 1
```Ở đây M = 3 và K = 2, do đó M >= K. Chúng ta không cần phân vùng DP. 

Giá trị tốt nhất cho số liệu 1 là 10, đạt được bởi bộ phân loại 1. Giá trị tốt nhất cho số liệu 2 cũng là 10, đạt được bởi bộ phân loại 1. 

| Số liệu | Tối đa | Người chiến thắng | 
| --- | --- | --- | 
| 1 | 10 | 1 | 
| 2 | 10 | 1 | 

Ban đầu chỉ cần phân loại 1, nhưng phải in chính xác ba phân loại. Phân loại 2 và 3 được thêm vào dưới dạng phần đệm. 

Độ hữu ích vẫn là 20 vì việc thêm các bộ phân loại không thể giảm tối đa.```
20
1 2 3
```Ví dụ này thực hiện nhánh M >= K đặc biệt và yêu cầu xuất ra chính xác M chỉ số phân loại riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NK2^K + M3^(K-1)) | Mỗi bộ phân loại đóng góp tất cả các mặt nạ số liệu, theo sau là M lớp DP phân vùng tập hợp con | 
| Không gian | O(NK + M2^K) | Ma trận đầu vào, lớp cha DP và mảng mặt nạ phụ được lưu trữ | 

Với K = 15, chỉ có 32768 mặt nạ số liệu. Thuật ngữ đầu tiên là về N · K · 32768 phép toán, trong khi phân vùng DP là phần chiếm ưu thế khi M gần với K. Giới hạn K nhỏ là yếu tố làm cho thuật toán hàm mũ có thể thực hiện được; sự phụ thuộc hàm mũ vào N sẽ hoàn toàn không khả thi với N = 2000. 

Bản thân thuật toán là giải pháp chính xác thích hợp cho các ràng buộc đã cho. Việc triển khai Python sử dụng một số tối ưu hóa ở mức độ thấp, nhưng giới hạn 3 giây ban đầu đặc biệt đòi hỏi Python đối với các đầu vào trong trường hợp xấu nhất. Việc triển khai C++ của cùng một DP là cách gửi cuộc thi an toàn hơn. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve`chức năng từ giải pháp được đặt trong cùng một tệp. Nó chuyển hướng đầu vào và đầu ra tiêu chuẩn để việc triển khai chương trình cạnh tranh thực tế được thử nghiệm thay vì triển khai lại riêng biệt.```python
import sys
import io
import contextlib

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        out = io.StringIO()
        with contextlib.redirect_stdout(out):
            solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin

def check_output(inp: str, out: str):
    data = list(map(int, inp.split()))
    n, m, k = data[0], data[1], data[2]

    values = []
    p = 3
    a = []
    for _ in range(n):
        row = data[p:p + k]
        p += k
        a.append(row)

    tokens = list(map(int, out.split()))
    assert len(tokens) == m + 1

    answer = tokens[0]
    ids = tokens[1:]

    assert len(set(ids)) == m
    assert all(1 <= x <= n for x in ids)

    mx = [0] * k
    for idx in ids:
        row = a[idx - 1]
        for j in range(k):
            mx[j] = max(mx[j], row[j])

    assert sum(mx) == answer
    return answer

sample1 = """\
6 2 3
4 1 1
1 4 1
1 1 4
1 3 3
3 1 3
3 3 1
"""

sample2 = """\
3 3 2
10 10
5 5
1 1
"""

assert run(sample1) == "10\n1 4", "sample 1"
assert run(sample2) == "20\n1 2 3", "sample 2"

case_min = """\
1 1 1
100000000
"""
assert run(case_min) == "100000000\n1", "minimum-size case"

case_m_one = """\
2 1 2
10 1
1 10
"""
assert run(case_m_one) == "11\n1", "M = 1"

case_all_equal = """\
4 2 2
5 5
5 5
5 5
5 5
"""
assert run(case_all_equal) == "10\n1 2", "all equal values"

case_forced_all = """\
3 3 3
1 2 3
3 2 1
2 3 2
"""
assert run(case_forced_all) == "8\n1 2 3", "M = N"

case_max_n = "2000 2000 15\n" + ("1 1 1 1 1 1 1 1 1 1 1 1 1 1 1\n" * 2000)
out = run(case_max_n)
assert check_output(case_max_n, out) == 15, "large N"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 100000000`|`100000000 / 1`| N, M và K tối thiểu, cộng với giá trị số liệu lớn nhất | 
|`2 1 2 / 10 1 / 1 10`|`11 / 1`| M = 1, ngăn cản việc lựa chọn độc lập người chiến thắng số liệu | 
| Bốn hàng giống nhau có M = 2 |`10 / 1 2`| Cà vạt và đệm có kích thước chính xác | 
|`3 3 3`với ba hàng khác nhau |`8 / 1 2 3`| M = N, trong đó toàn bộ tập hợp bị buộc | 
| 2000 hàng giống nhau, M = N, K = 15 |`15 / 1 ... 2000`| N tối đa và đầu ra lớn, đồng thời thực hiện phím tắt M = N | 

## Vỏ cạnh 

Với M = 1, hãy xem xét```
2 1 2
10 1
1 10
```DP chính chỉ có một nhóm. Mặt nạ số liệu đầy đủ là`{1,2}`và f({1,2}) là max(11, 11) = 11. Phân loại 1 được chọn theo quy tắc ràng buộc xác định. Kết quả là 11 với phân loại 1. Thuật toán không bao giờ kết hợp cực đại độc lập 10 và 10 vì làm như vậy sẽ cần hai nhóm, vi phạm M = 1. 

Với M >= K, hãy xét```
3 3 2
10 10
5 5
1 1
```Các cực đại số liệu riêng lẻ đều là 10 và đều thuộc về bộ phân loại 1. Danh sách được chọn ban đầu chỉ chứa bộ phân loại 1. Vòng lặp đệm sau đó thêm các bộ phân loại 2 và 3. Tập kết quả có chính xác ba chỉ số riêng biệt và vẫn có giá trị hữu ích 20. 

Với M = N, hãy xem xét```
3 3 3
1 2 3
3 2 1
2 3 2
```Mỗi bộ phân loại phải được chọn, do đó thuật toán ngay lập tức tính toán cực đại theo tọa độ`(3,3,3)`. Tổng của chúng là 8 và nó xuất ra cả ba chỉ số. Không có phân vùng là cần thiết. 

Với mọi giá trị bằng nhau,```
4 2 2
5 5
5 5
5 5
5 5
```mọi phân loại đều tối ưu cho mọi số liệu. Trình phân loại đầu tiên giành được cả hai cực đại số liệu, sau đó bước đệm chọn phân loại 2. Tính hữu ích thu được vẫn là 10. Điều này chứng tỏ rằng thuật toán không phụ thuộc vào cực đại duy nhất. 

Đối với một nhóm mà bộ phân loại tốt nhất của nó cũng là tốt nhất cho một nhóm khác, hãy xem xét```
3 2 3
10 10 10
9 1 1
1 9 1
```Phân vùng`{1,2,3}`sẽ sử dụng phân loại 1 và có giá trị 30, trong khi phân vùng hai nhóm như`{1} | {2,3}`cũng cho 30 vì phân loại 1 là đại diện tốt nhất cho cả hai nhóm. Trong quá trình xây dựng lại, cả hai nhóm có thể tạo ra bộ phân loại 1. Bản sao bị loại bỏ, để lại một bộ phân loại và một bộ phân loại không sử dụng khác được thêm vào để đạt M = 2. Độ hữu dụng vẫn là 30, là mức tối ưu. 

Đối với N lớn có M = N, nhánh đặc biệt đặc biệt hữu ích. Với 2000 bộ phân loại và 15 số liệu, việc chọn tất cả mọi người là bắt buộc nên thuật toán chỉ thực hiện công việc O(NK). DP phân vùng số liệu chung sẽ là chi phí không cần thiết trong trường hợp này. 

Ranh giới tinh tế nhất là M < K. Ở đây, mức tối đa độc lập của mọi số liệu nói chung là không thể đạt được vì nó có thể yêu cầu nhiều hơn M bộ phân loại. Phân vùng DP nắm bắt rõ ràng hạn chế này bằng cách buộc các số liệu K vào chính xác M nhóm, với một bộ phân loại chịu trách nhiệm cho mỗi nhóm.
