---
title: "CF 105201M - Thử nghiệm đảo ngược"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm độc lập, mỗi trường hợp bao gồm một mảng. Nhiệm vụ không phải là tính toán số lần đảo ngược mà là tìm đoạn liền kề ngắn nhất của mảng bảo toàn chính xác số lần đảo ngược như toàn bộ mảng."
date: "2026-06-27T02:49:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105201
codeforces_index: "M"
codeforces_contest_name: "IME++ Open Contest 2024"
rating: 0
weight: 105201
solve_time_s: 76
verified: false
draft: false
---

[CF 105201M - Thử nghiệm đảo ngược](https://codeforces.com/problemset/problem/105201/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm độc lập, mỗi trường hợp bao gồm một mảng. Nhiệm vụ không phải là tính toán số lần đảo ngược mà là tìm đoạn liền kề ngắn nhất của mảng bảo toàn chính xác số lần đảo ngược như toàn bộ mảng. 

Sự đảo ngược được xác định bởi một cặp vị trí trong đó giá trị bên trái lớn hơn giá trị bên phải. Nếu chúng tôi loại bỏ các phần tử khỏi các đầu, chúng tôi sẽ thay đổi cặp nào tồn tại, bởi vì bất kỳ sự đảo ngược nào chạm vào phần tử bị loại bỏ sẽ biến mất hoàn toàn. Mục tiêu là thu nhỏ mảng từ cả hai phía càng nhiều càng tốt trong khi vẫn giữ nguyên tổng số lần đảo ngược. 

Ràng buộc chính là tổng kích thước mảng trong tất cả các trường hợp thử nghiệm lên tới 10^6. Điều này loại trừ mọi cách tiếp cận bậc hai cho mỗi trường hợp thử nghiệm. Ngay cả giải pháp O(n log n) cho mỗi trường hợp thử nghiệm cũng có thể được chấp nhận, nhưng bất cứ điều gì cố gắng tính toán lại số lượng nghịch đảo cho nhiều mảng con ứng cử viên sẽ quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi mảng hoàn toàn không giảm. Trong tình huống đó, không có sự đảo ngược nào cả, và bất kỳ mảng con nào cũng không có sự đảo ngược nào. Vấn đề xác định rằng mảng con hợp lệ nhỏ nhất trong trường hợp này có kích thước bằng 0, nghĩa là chúng ta được phép xóa mọi thứ. 

Ví dụ: nếu mảng là [1, 2, 3, 4] thì không có sự đảo ngược. Câu trả lời là 0 vì mảng con trống giữ nguyên số lượng đảo ngược. 

Một trường hợp cạnh quan trọng khác là khi chỉ có một phần tử tham gia vào tất cả các phép nghịch đảo. Nếu chúng ta giả định không chính xác rằng chúng ta luôn có thể giữ ít nhất một phần tử, chúng ta có thể bỏ lỡ rằng câu trả lời đúng có thể bằng 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi mảng con có thể, tính số lần đảo ngược của nó và so sánh nó với mảng ban đầu. Tính toán nghịch đảo cho một mảng bằng cách sử dụng cây Fenwick hoặc sắp xếp hợp nhất tốn O(n log n) và có các mảng con O(n^2), do đó, điều này trở thành O(n^3 log n) trong trường hợp xấu nhất, hoàn toàn không khả thi đối với n lên đến 10^6. 

Ngay cả khi chúng tôi cố gắng tối ưu hóa bằng cách sửa ranh giới bên trái và mở rộng ranh giới bên phải trong khi duy trì số lần đảo ngược tăng dần, chúng tôi vẫn phải đối mặt với vấn đề việc loại bỏ các phần tử sẽ làm thay đổi mối quan hệ cặp tổng thể theo cách không cục bộ. Không có bản cập nhật cửa sổ trượt rõ ràng nào cho số lượng đảo ngược vì việc loại bỏ một phần tử sẽ phá hủy tất cả các đảo ngược liên quan đến nó. 

Quan sát quan trọng là chúng ta không cần phải theo dõi số lần đảo ngược một cách linh hoạt. Thay vào đó, chúng tôi phân loại từng phần tử dựa trên việc nó có tham gia vào ít nhất một phép đảo ngược trong mảng ban đầu hay không. 

Nếu một phần tử không bao giờ tham gia vào bất kỳ sự đảo ngược nào thì việc loại bỏ nó sẽ không ảnh hưởng đến số lần đảo ngược. Nếu nó tham gia vào ít nhất một lần đảo ngược thì việc loại bỏ nó sẽ phá hủy ít nhất một lần đảo ngược, do đó tổng số lần đảo ngược sẽ giảm đi đáng kể. Do đó, bất kỳ mảng con hợp lệ nào cũng phải chứa mọi phần tử tham gia vào một phép đảo ngược nào đó. 

Điều này làm giảm vấn đề tìm phân đoạn nhỏ nhất bao gồm tất cả các vị trí “hoạt động đảo ngược”. 

Một phần tử được kích hoạt đảo ngược nếu tồn tại phần tử nhỏ hơn ở bên phải hoặc phần tử lớn hơn ở bên trái. Điều này có thể được kiểm tra bằng cách sử dụng tiền tố tối đa và hậu tố tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3 log n) | O(n) | Quá chậm | 
| Tiền tố/Hậu tố cực đoan | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi trường hợp thử nghiệm, hãy tính một mảng tiền tố tối đa trong đó tiền tố_max[i] là giá trị tối đa trong a[1..i]. Điều này cho phép chúng ta nhanh chóng xác định xem a[i] có lớn ít nhất bằng mọi thứ ở bên trái của nó hay không. 
2. Tính toán một mảng hậu tố tối thiểu trong đó hậu tố_min[i] là giá trị tối thiểu trong a[i..n]. Điều này cho chúng ta biết liệu a[i] có nhỏ nhất bằng mọi thứ ở bên phải nó hay không. 
3. Đối với mỗi vị trí i, hãy quyết định xem nó có tham gia vào bất kỳ sự đảo ngược nào không. Một vị trí có thể bị loại bỏ một cách an toàn nếu tất cả các phần tử ở bên trái không lớn hơn nó và tất cả các phần tử ở bên phải không nhỏ hơn nó. Về mặt hình thức, điều này có nghĩa là prefix_max[i-1] ≤ a[i] và a[i] ≤ suffix_min[i+1]. 
4. Đánh dấu tất cả các vị trí vi phạm điều kiện này. Đây chính xác là những phần tử phải có trong mọi mảng con hợp lệ. 
5. Tìm chỉ số ngoài cùng bên trái và ngoài cùng bên phải được đánh dấu. Chúng xác định phân đoạn nhỏ nhất chứa tất cả các phần tử cần thiết. 
6. Nếu không có vị trí nào được đánh dấu, mảng có số lần đảo ngược bằng 0 và câu trả lời là 0. Ngược lại, xuất ra ngoài cùng bên phải − ngoài cùng bên trái + 1. 

### Tại sao nó hoạt động 

Mỗi phép đảo ngược bao gồm hai vị trí và mỗi lần đảo ngược sẽ biến mất nếu một trong hai điểm cuối bị loại bỏ. Nếu một phần tử tham gia vào ít nhất một lần đảo ngược, việc xóa phần tử đó sẽ loại bỏ hoàn toàn nghịch đảo đó khỏi số đếm. Ngược lại, các phần tử không bao giờ là một phần của bất kỳ phép đảo ngược nào sẽ không đóng góp vào số lần đảo ngược và có thể bị xóa tự do mà không ảnh hưởng đến tổng số. 

Các điều kiện tối đa và hậu tố tối thiểu của tiền tố mô tả chính xác các phần tử không thể tạo thành sự đảo ngược với bất kỳ phần tử nào khác. Mọi thứ bên ngoài tập hợp đó đều bị buộc phải đưa vào câu trả lời và mảng con hợp lệ nhỏ nhất chính xác là khoảng thời gian tối thiểu bao trùm tất cả các vị trí bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if n == 0:
            out.append("0")
            continue

        pref = [0] * n
        suff = [0] * n

        pref[0] = a[0]
        for i in range(1, n):
            pref[i] = max(pref[i - 1], a[i])

        suff[n - 1] = a[n - 1]
        for i in range(n - 2, -1, -1):
            suff[i] = min(suff[i + 1], a[i])

        L, R = n, -1

        for i in range(n):
            left_ok = (i == 0 or pref[i - 1] <= a[i])
            right_ok = (i == n - 1 or a[i] <= suff[i + 1])

            if not (left_ok and right_ok):
                L = min(L, i)
                R = max(R, i)

        if R == -1:
            out.append("0")
        else:
            out.append(str(R - L + 1))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xây dựng tiền tố cực đại và hậu tố cực tiểu để mỗi vị trí có thể được phân loại theo thời gian không đổi. Phần tinh tế duy nhất là xử lý ranh giới một cách chính xác: khi kiểm tra bên trái của phần tử đầu tiên hoặc bên phải của phần tử cuối cùng, chúng tôi coi các điều kiện đó là tự động được thỏa mãn. 

Khoảng thời gian cuối cùng được xây dựng bằng cách theo dõi các chỉ số tối thiểu và tối đa của tất cả các yếu tố cần thiết để duy trì. Nếu không có phần tử nào như vậy tồn tại thì toàn bộ mảng có thể bị xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [4, 9, 9, 0, 2] 

Chúng tôi tính toán mức tối đa tiền tố và mức tối thiểu hậu tố. 

| tôi | một [tôi] | tiền tố_max[i-1] | hậu tố_min[i+1] | giữ đảo ngược? | đánh dấu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | - | 0 | không | vâng | 
| 1 | 9 | 4 | 0 | không | vâng | 
| 2 | 9 | 9 | 0 | không | vâng | 
| 3 | 0 | 9 | 2 | không | vâng | 
| 4 | 2 | 9 | - | không | vâng | 

Mọi phần tử đều tham gia vào một số nghịch đảo, vì vậy phân đoạn nhỏ nhất là toàn bộ mảng. Câu trả lời là 5. 

Điều này xác nhận trường hợp cấu trúc hoàn toàn bị vướng víu và không thể cắt tỉa được. 

### Ví dụ 2 

Mảng đầu vào: [1, 2, 3, 4, 5] 

| tôi | một [tôi] | tiền tố_max[i-1] | hậu tố_min[i+1] | giữ đảo ngược? | đánh dấu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | 2 | vâng | không | 
| 1 | 2 | 1 | 3 | vâng | không | 
| 2 | 3 | 2 | 4 | vâng | không | 
| 3 | 4 | 3 | 5 | vâng | không | 
| 4 | 5 | 4 | - | vâng | không | 

Không có phần tử nào tham gia vào bất kỳ phép đảo ngược nào, vì vậy R vẫn giữ nguyên -1 và đầu ra là 0. 

Điều này thể hiện trường hợp đặc biệt khi số lượng đảo ngược bằng 0 và câu trả lời đúng là một mảng con trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm tính toán các mảng tiền tố và hậu tố theo thời gian tuyến tính và quét một lần để xác định giới hạn | 
| Không gian | O(n) | Lưu trữ mảng tiền tố và hậu tố | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 10^6, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# provided samples (as formatted)
assert run("2\n5\n4 9 9 0 2\n5\n1 2 3 4 5") == "5\n0", "samples"

# all equal
assert run("1\n4\n7 7 7 7") == "0", "all equal"

# single inversion pair
assert run("1\n2\n2 1") == "2", "single inversion"

# already fully inverted structure
assert run("1\n3\n3 2 1") == "3", "full inversion"

# minimal size
assert run("1\n1\n10") == "0", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | không tham gia đảo ngược | 
| [2, 1] | 2 | trường hợp đảo ngược không tầm thường nhỏ nhất | 
| [3, 2, 1] | 3 | cấu trúc đảo ngược hoàn toàn | 
| phần tử đơn | 0 | quy ước mảng con trống | 

## Vỏ cạnh 

Khi mảng tăng nghiêm ngặt, mọi phần tử đều thỏa mãn các điều kiện tiền tố và hậu tố nên không có phần tử nào được đánh dấu. Thuật toán trả về chính xác một khoảng trống vì R vẫn ở mức -1. 

Khi mảng giảm nghiêm ngặt, mọi phần tử đều vi phạm ít nhất một điều kiện, do đó khoảng được đánh dấu sẽ trở thành mảng đầy đủ. Thuật toán trả về n, khớp với thực tế là mọi phần tử đều tham gia vào quá trình đảo ngược. 

Khi các bản sao xuất hiện, chúng không tạo ra sự đảo ngược giữa các giá trị bằng nhau nhưng vẫn tương tác chính xác với các bất đẳng thức nghiêm ngặt trong kiểm tra tiền tố và hậu tố. Các phần tử bằng mức tối đa hoặc tối thiểu biên chỉ được coi là an toàn nếu chúng thực sự không thể hình thành sự đảo ngược nghiêm ngặt ở cả hai phía.
