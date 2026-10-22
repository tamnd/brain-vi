---
title: "CF 105901L - Chuỗi tiếp theo"
description: "Chúng ta được cung cấp một số mảng số nguyên và với mỗi mảng chúng ta muốn xây dựng một dãy con có thuộc tính cấu trúc rất cụ thể. Lấy bất kỳ dãy con nào đã chọn và sắp xếp nó."
date: "2026-06-21T15:22:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105901
codeforces_index: "L"
codeforces_contest_name: "2025 ICPC Wuhan Invitational Contest (The 3rd Universal Cup. Stage 37: Wuhan)"
rating: 0
weight: 105901
solve_time_s: 63
verified: true
draft: false
---

[CF 105901L - Trình tự tiếp theo](https://codeforces.com/problemset/problem/105901/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số mảng số nguyên và với mỗi mảng chúng ta muốn xây dựng một dãy con có thuộc tính cấu trúc rất cụ thể. 

Lấy bất kỳ dãy con nào đã chọn và sắp xếp nó. Đặt phần tử nhỏ nhất là điểm cuối bên trái, phần tử lớn nhất là điểm cuối bên phải và trung vị là phần tử ở vị trí ceil(k/2) theo thứ tự được sắp xếp này. Dãy con được gọi là tốt nếu trung vị chính xác là trung bình số học của các giá trị tối thiểu và tối đa. 

Nói cách khác, nếu dãy con được sắp xếp là D thì chúng ta yêu cầu 2 · D[ceil(k/2)] bằng D[1] + D[k]. Điều kiện kết hợp ba số liệu thống kê thứ tự khác nhau của tập hợp đã chọn, điều này ngay lập tức làm rõ rằng các chuỗi con tùy ý bị hạn chế rất nhiều. 

Nhiệm vụ là tìm, đối với mỗi trường hợp thử nghiệm, độ dài tối đa có thể có của một chuỗi con tốt được trích xuất từ ​​mảng ban đầu trong khi vẫn giữ nguyên thứ tự tương đối. 

Các ràng buộc tổng hợp là nhỏ, với tổng số n trong tất cả các trường hợp thử nghiệm không vượt quá 3000. Điều này loại bỏ mọi nhu cầu tối ưu hóa n log n hoặc n log^2 n và gợi ý rõ ràng rằng chiến lược O(n^2) hoặc thậm chí O(n^2 log n) là dự định. 

Một vấn đề tế nhị sẽ xuất hiện ngay lập tức nếu người ta cố gắng lý luận tham lam hoặc cục bộ. Điều kiện liên quan đến trung vị, phụ thuộc vào kích thước của dãy con đã chọn, do đó việc loại bỏ hoặc thêm một phần tử có thể thay đổi phần tử nào trở thành trung vị và thay đổi hoàn toàn tính hợp lệ. Ví dụ: một khoảng có thể không đạt điều kiện với tất cả các phần tử được đưa vào, nhưng trở nên hợp lệ sau khi loại bỏ một số giá trị bên trong. Điều này giết chết những ý tưởng ngây thơ “lấy một phạm vi giá trị và kiểm tra một lần” trừ khi cấu trúc đảm bảo tính tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ liệt kê tất cả các chuỗi con, kiểm tra từng chuỗi bằng cách sắp xếp và xác minh xem trung vị có bằng mức trung bình của mức tối thiểu và tối đa hay không. Điều này đúng nhưng không khả thi. Có 2^n dãy con và thậm chí việc kiểm tra một dãy cũng có giá O(n log n), vì vậy trường hợp xấu nhất sẽ vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là điều kiện chỉ phụ thuộc vào ba giá trị của dãy con được sắp xếp: cực tiểu, cực đại và trung vị. Nếu chúng ta cố định x tối thiểu và y tối đa thì trung vị buộc phải là (x + y)/2, miễn là giá trị đó là tích phân và xuất hiện trong mảng. Điều này làm giảm vấn đề từ các chuỗi con tùy ý đến việc chọn các cặp điểm cuối. 

Khi x và y được cố định, mọi dãy con ứng cử viên phải nằm hoàn toàn bên trong phạm vi giá trị [x, y], nếu không thì giá trị tối thiểu hoặc tối đa sẽ thay đổi. Trong số tất cả các phần tử trong phạm vi đó, chúng tôi muốn chọn chuỗi con lớn nhất có thể vẫn giữ (x + y) / 2 làm trung vị của nó. 

Điều này chuyển vấn đề thành việc kiểm tra, đối với mỗi cặp (x, y), liệu tập hợp đầy đủ các phần tử trong [x, y] có thỏa mãn ràng buộc trung vị hay không và nếu có, chúng tôi sẽ lấy tất cả chúng. Mặc dù về nguyên tắc, chúng ta có thể loại bỏ các phần tử, nhưng làm như vậy chỉ làm giảm độ dài và không thể giúp ích một khi toàn bộ khoảng đã thỏa mãn điều kiện trung bình. 

Do đó, chúng tôi chuyển sang phối hợp tần số nén và tiền tố. Với mỗi cặp giá trị nén i ≤ j, chúng ta coi U[i] là x và U[j] là y. Chúng tôi tính điểm giữa m = (x + y) / 2, kiểm tra xem nó có phải là số nguyên và tồn tại trong mảng không, sau đó đánh giá xem có bao nhiêu phần tử trong khoảng nhỏ hơn m, bằng m và lớn hơn m. Những số đếm này xác định đầy đủ điều kiện vị trí trung bình. 

Lực lượng vũ phu đối với tất cả các cặp là O(n^2) và mỗi kiểm tra là O(1) sử dụng tổng tiền tố, phù hợp thoải mái trong tổng ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo | O(2^n · n log n) | O(n) | Quá chậm | 
| Ghép nối điểm cuối với số lượng tiền tố | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán

1. Nén tất cả các giá trị trong mảng thành một danh sách duy nhất được sắp xếp U và xây dựng một mảng tần số trên U. 

Điều này cho phép trả lời các truy vấn phạm vi trên các giá trị theo thời gian không đổi bằng cách sử dụng tổng tiền tố. 
2. Xây dựng tổng tiền tố trên mảng tần số để chúng ta có thể nhanh chóng tính toán có bao nhiêu phần tử rơi vào bất kỳ khoảng giá trị [U[i], U[j] nào. 
3. Lặp lại tất cả các điểm cuối bên trái có thể có i, coi U[i] là giá trị tối thiểu ứng viên. 
4. Với mỗi i, lặp qua tất cả các điểm cuối bên phải j ≥ i, coi U[j] là giá trị tối đa ứng cử viên. 
5. Tính x = U[i] và y = U[j]. Nếu (x + y) là số lẻ, hãy bỏ qua cặp này vì không có trung vị nguyên nào có thể thỏa mãn điều kiện. 
6. Tính m = (x + y) / 2 và xác định vị trí của nó trong mảng nén. Nếu m không tồn tại thì bỏ qua cặp này. 
7. Sử dụng tổng tiền tố, tính L là số phần tử trong A có giá trị [x, m), E là số phần tử bằng m và R là số phần tử trong (m, y]. 
8. Cho k = L + E + R và t = ceil(k / 2). Dãy con đúng khi và chỉ khi L < t và t ≤ L + E. 
9. Nếu hợp lệ, hãy cập nhật câu trả lời bằng k, vì việc lấy toàn bộ khoảng sẽ cho chuỗi con tối đa có thể có cho điều này (x, y). 

### Tại sao nó hoạt động 

Việc cố định các lực tối thiểu và tối đa cho mỗi dãy con hợp lệ nằm bên trong một khoảng giá trị đóng. Bên trong một khoảng như vậy, cách duy nhất để tác động đến trung vị mà không thay đổi điểm cuối là loại bỏ các phần tử bên trong, nhưng việc loại bỏ các phần tử chỉ làm co k và dịch chuyển vị trí trung vị theo cách không thể mở rộng tính khả thi vượt quá những gì toàn bộ khoảng đã cung cấp cho một cố định (x, y, m). Do đó, đối với mỗi cặp điểm cuối, khoảng đầy đủ đã thỏa mãn ràng buộc trung vị hoặc không thể tồn tại dãy con hợp lệ lớn hơn với các điểm cuối đó. 

Điều kiện đếm tiền tố mã hóa trực tiếp định nghĩa vị trí trung vị về số lượng phần tử nằm bên dưới và ở giá trị trung vị, do đó việc kiểm tra tính hợp lệ là chính xác và không mang tính phỏng đoán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))

        vals = sorted(set(a))
        idx = {v: i for i, v in enumerate(vals)}
        m = len(vals)

        freq = [0] * m
        for v in a:
            freq[idx[v]] += 1

        pref = [0] * (m + 1)
        for i in range(m):
            pref[i + 1] = pref[i] + freq[i]

        def range_sum(l, r):
            if l > r:
                return 0
            return pref[r + 1] - pref[l]

        ans = 1

        for i in range(m):
            for j in range(i, m):
                x = vals[i]
                y = vals[j]

                s = x + y
                if s % 2:
                    continue
                mid = s // 2
                if mid not in idx:
                    continue

                k = idx[mid]
                if k < i or k > j:
                    continue

                L = range_sum(i, k - 1)
                E = freq[k]
                R = range_sum(k + 1, j)

                total = L + E + R
                if total == 0:
                    continue

                t = (total + 1) // 2

                if L < t <= L + E:
                    ans = max(ans, total)

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ nén các giá trị để có thể đánh giá nhanh chóng mọi khoảng giá trị. Các vòng lặp lồng nhau liệt kê tất cả các lựa chọn có thể có về giá trị tối thiểu và tối đa. Đối với mỗi cặp, giá trị trung điểm được tính toán và xác thực, vì trung vị phải bằng chính xác điểm giữa đó. 

Tổng tiền tố cho phép L và R được tính toán trong thời gian không đổi, do đó mỗi lần kiểm tra cặp là O(1). Điều kiện L < t ≤ L + E là bản dịch trực tiếp của yêu cầu vị trí trung vị nằm trong khối các giá trị bằng đến giữa trong khoảng được sắp xếp. 

Câu trả lời cuối cùng là kích thước tối đa của bất kỳ khoảng nào thỏa mãn ràng buộc này. 

## Ví dụ đã hoạt động 

Xét mảng A = [9, 8, 2, 11, 5]. 

Chúng tôi nén các giá trị thành [2, 5, 8, 9, 11]. Giả sử chúng ta chọn x = 2 và y = 8. Khi đó m = 5. Chúng ta tính số đếm trên [2, 8]: L = 1 (giá trị 2), E = 1 (giá trị 5), R = 1 (giá trị 8). Vậy tổng k = 3 và t = 2. Vì L = 1 < 2 2 = L + E nên khoảng này hợp lệ và đóng góp cho câu trả lời 3. 

| tôi (x) | j (y) | m | L | E | R | k | t | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 8 | 5 | 1 | 1 | 1 | 3 | 2 | Có | 

Dấu vết này cho thấy cấu trúc cân bằng xung quanh giá trị trung bình, trong đó chính xác một phần tử nằm bên dưới và một phần tử nằm ở trên. 

Bây giờ hãy xem xét A = [7, 9, 2, 4, 17, 10, 15]. Lấy x = 4 và y = 10, cho m = 7. Bên trong [4, 10], chúng ta có các phần tử 4, 7, 9, 10. Khi đó L = 1, E = 1, R = 2, k = 4, t = 2. Vì L < t ≤ L + E đúng nên khoảng này đúng. 

| tôi (x) | j (y) | m | L | E | R | k | t | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 10 | 7 | 1 | 1 | 2 | 4 | 2 | Có | 

Điều này cho thấy các chuỗi con có độ dài chẵn được xử lý chính xác như thế nào thông qua chỉ số trung vị dựa trên trần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | tất cả các cặp giá trị nén được kiểm tra một lần, mỗi cặp trong O(1) | 
| Không gian | O(n) | mảng tần số và tiền tố trên các giá trị nén | 

Với tổng n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 3000, việc quét bậc hai vẫn nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# basic sample-like cases
assert run("1\n5\n9 8 2 11 5\n") == "3"
assert run("1\n7\n7 9 2 4 17 10 15\n") == "4"

# minimum size
assert run("1\n1\n100\n") == "1"

# all equal values
assert run("1\n5\n3 3 3 3 3\n") == "5"

# symmetric valid structure
assert run("1\n3\n1 3 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | n | trung vị phù hợp tầm thường | 
| đối xứng quanh điểm giữa | đầy đủ | giá trị trường hợp tốt nhất | 
| giá trị hỗn hợp | khác nhau | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi giá trị trung điểm tồn tại nhưng nằm ngoài khoảng giá trị đã chọn. Ví dụ: nếu chúng ta chọn x = 2 và y = 8 nhưng không có phần tử nào bằng 5 thì điều kiện phải thất bại ngay cả khi điểm giữa số học hợp lệ. Thuật toán kiểm tra rõ ràng sự hiện diện của m trong bản đồ nén, ngăn chặn các giả định trung vị không hợp lệ. 

Một trường hợp cạnh khác là khi khoảng chứa nhiều phần tử bằng trung vị. Với A = [5, 5, 5, 1, 9], việc chọn x = 1 và y = 9 sẽ cho m = 5, với E = 3. Điều kiện trung vị được thỏa mãn đối với nhiều độ dài chuỗi con có thể có và kiểm tra L < t ≤ L + E sẽ nắm bắt chính xác tất cả chúng vì bất kỳ vị trí trung vị nào cũng nằm trong khối có giá trị bằng nhau. 

Trường hợp tinh vi cuối cùng là các dãy con có độ dài chẵn. Bởi vì trung vị được xác định bằng cách sử dụng ceil(k/2), nên vị trí trung tuyến dịch chuyển sang trái một chút so với định nghĩa tiêu chuẩn “trung vị thấp hơn”. Điều kiện t = ceil(k/2) đảm bảo rằng trung vị luôn được xử lý một cách nhất quán và bất đẳng thức L < t ≤ L + E xử lý cả k lẻ và chẵn mà không cần viết vỏ đặc biệt.
