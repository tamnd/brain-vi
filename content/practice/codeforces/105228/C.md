---
title: "CF 105228C - Hậu tố"
description: "Chúng ta được cung cấp một tập hợp các từ trong từ điển cố định, mỗi từ rất ngắn và sau đó là một số lượng lớn các truy vấn. Mỗi truy vấn cung cấp một từ và một thứ hạng. Đối với một từ truy vấn, chúng tôi so sánh nó với mọi từ trong từ điển bằng cách sử dụng kết hợp hậu tố."
date: "2026-06-24T16:20:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105228
codeforces_index: "C"
codeforces_contest_name: "SanSi Cup 2023"
rating: 0
weight: 105228
solve_time_s: 312
verified: false
draft: false
---

[CF 105228C - Hậu tố](https://codeforces.com/problemset/problem/105228/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các từ trong từ điển cố định, mỗi từ rất ngắn và sau đó là một số lượng lớn các truy vấn. Mỗi truy vấn cung cấp một từ và một thứ hạng. 

Đối với một từ truy vấn, chúng tôi so sánh nó với mọi từ trong từ điển bằng cách sử dụng kết hợp hậu tố. Một từ trong từ điển được coi là có liên quan nếu nó có ít nhất một hậu tố với từ truy vấn. Trong số tất cả các kết quả trùng khớp như vậy, trước tiên chúng tôi xác định độ dài hậu tố tối đa có thể xuất hiện giữa từ truy vấn và bất kỳ từ trong từ điển nào. Chỉ những từ trong từ điển đạt được độ dài hậu tố tối đa này mới được giữ lại. Từ những thứ này, chúng tôi sắp xếp các từ theo từ điển và chọn từ thứ k. Câu trả lời không phải là từ đó mà là vị trí ban đầu của nó trong danh sách đầu vào. Nếu không có đủ từ trong nhóm đã chọn, câu trả lời là -1. 

Chi tiết cấu trúc quan trọng là các từ cực kỳ ngắn, độ dài tối đa là 5. Điều đó ngay lập tức hạn chế số lượng hậu tố riêng biệt có thể xuất hiện, bởi vì mỗi từ chỉ đóng góp một số lượng hậu tố không đổi. 

Một cách giải thích ngây thơ sẽ cố gắng so sánh từng từ truy vấn với tất cả các từ trong từ điển và tính toán trực tiếp các hậu tố phù hợp. Với tối đa 10^5 từ trong từ điển và 10^5 truy vấn, điều đó sẽ quá chậm ngay cả khi mỗi lần so sánh ngắn. 

Vấn đề tế nhị thứ hai là “kết quả phù hợp nhất” được xác định trên toàn cầu cho mỗi truy vấn chứ không phải cho mỗi từ một cách độc lập. Một từ trong từ điển khớp với hậu tố ngắn hơn sẽ không liên quan nếu một từ khác khớp với hậu tố dài hơn. 

Một trường hợp lỗi đơn giản xuất hiện khi tồn tại nhiều độ dài hậu tố. 

Ví dụ: nếu các từ trong từ điển là`["aba", "caba"]`và truy vấn là`"xxcaba"`, cả hai từ đều có hậu tố phù hợp`"aba"`nhưng chỉ`"caba"`khớp với hậu tố có độ dài 4. Câu trả lời đúng phải bỏ qua hoàn toàn tất cả các kết quả khớp có độ dài 3. 

Một cạm bẫy khác là quên thứ tự từ điển sau khi lọc theo độ dài hậu tố. Hai từ có thể khớp với độ dài hậu tố tốt nhất nhưng câu trả lời phụ thuộc vào thứ tự sắp xếp chứ không phải thứ tự nhập. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ trực tiếp so sánh từng từ truy vấn với mọi từ trong từ điển và tính toán hậu tố chung dài nhất của chúng bằng cách quét từ cuối tối đa 5 ký tự. Vì mỗi so sánh có chi phí O(5), nên một truy vấn có chi phí O(5n) và tất cả các truy vấn có chi phí O(5nq), theo thứ tự 10^10 thao tác. Điều này vượt xa giới hạn thời gian. 

Ràng buộc các từ có độ dài tối đa là 5 sẽ thay đổi hoàn toàn cấu trúc. Mỗi từ trong từ điển chỉ đóng góp một tập hợp nhỏ các hậu tố: ký tự cuối cùng, hai ký tự cuối cùng, v.v. cho đến từ đầy đủ. Nếu đảo ngược suy nghĩ của mình, thay vì so sánh các truy vấn với tất cả các từ, chúng ta có thể nhóm trước các từ trong từ điển theo hậu tố. 

Đối với mỗi từ trong từ điển, chúng tôi tạo ra tất cả các hậu tố của nó và lưu trữ từ đó trong một nhóm được khóa bằng chuỗi hậu tố đó. Mỗi nhóm đại diện cho tất cả các từ trong từ điển có chung hậu tố đó. Nếu một truy vấn kết thúc bằng cùng một hậu tố, chúng tôi có thể truy xuất ngay lập tức tất cả các từ phù hợp với độ dài hậu tố chính xác đó. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào hậu tố dài nhất tồn tại trong cấu trúc được tính toán trước này. Vì vậy, đối với mỗi truy vấn, chúng tôi kiểm tra hậu tố của nó từ dài nhất đến ngắn nhất cho đến khi tìm thấy nhóm không trống. Nhóm đó chính xác là tập hợp các từ đạt được độ dài kết hợp hậu tố tối đa có thể. Sau đó, chúng tôi sắp xếp nhóm đó theo từ điển một lần trong quá trình tiền xử lý để mỗi truy vấn có thể lập chỉ mục trực tiếp vào nhóm đó. 

Điều này làm giảm mỗi truy vấn để kiểm tra tối đa 5 hậu tố và sau đó thực hiện lựa chọn thứ k đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq · 5) | O(1) | Quá chậm | 
| Nhóm hậu tố được tính toán trước | O((n + q) · 5 + Σ sắp xếp) | O(n · 5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi từ trong từ điển, hãy tạo tất cả các hậu tố của nó bằng cách lấy ký tự cuối cùng, hai ký tự cuối cùng, v.v. cho đến từ đầy đủ. Lưu trữ chỉ mục gốc của từ bên trong bản đồ được khóa theo từng hậu tố. Điều này xây dựng quyền truy cập trực tiếp từ bất kỳ chuỗi hậu tố nào tới tất cả các từ có chứa nó. 
2. Sau khi xử lý tất cả các từ trong từ điển, hãy sắp xếp danh sách các từ được lưu trữ trong mỗi nhóm hậu tố theo từ điển của chính từ đó. Chúng ta giữ cả từ và chỉ mục gốc của nó để sau này có thể xuất ra vị trí cần thiết. 
3. Đối với mỗi từ truy vấn, hãy xem xét các hậu tố của nó theo thứ tự độ dài giảm dần. Bắt đầu từ từ đầy đủ và loại bỏ dần ký tự đầu tiên cho đến khi chỉ còn lại một ký tự. 
4. Đối với mỗi hậu tố, hãy kiểm tra xem nó có tồn tại trong bản đồ được tính toán trước hay không. Hậu tố đầu tiên được tìm thấy tương ứng với độ dài hậu tố tối đa có thể được chia sẻ với bất kỳ từ trong từ điển nào. 
5. Nếu không tìm thấy hậu tố nào trong bản đồ, hãy xuất -1 vì không có từ trong từ điển nào chia sẻ bất kỳ hậu tố nào với truy vấn. 
6. Nếu không, hãy lấy thùng tương ứng. Nếu kích thước của nó nhỏ hơn k, xuất ra -1. Nếu nó đủ lớn, hãy chọn phần tử thứ k theo thứ tự từ điển và xuất chỉ mục gốc được lưu trữ của nó. 

Tính đúng đắn xuất phát từ thực tế là các hậu tố được kiểm tra theo thứ tự độ dài giảm dần. Trận đấu đầu tiên đảm bảo tính tối đa, do đó, không còn trận đấu hậu tố nào có thể tồn tại cho bất kỳ từ trong từ điển nào ngoài nhóm đó. Tất cả các từ trong nhóm đó đều có cùng độ dài hậu tố đó và bất kỳ từ nào khớp với hậu tố ngắn hơn đều không liên quan theo định nghĩa của vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    words = []
    buckets = {}

    for i in range(n):
        w = input().strip()
        words.append(w)
        L = len(w)
        for j in range(L):
            suf = w[j:]
            if suf not in buckets:
                buckets[suf] = []
            buckets[suf].append((w, i + 1))

    for suf in buckets:
        buckets[suf].sort(key=lambda x: x[0])

    q = int(input())
    for _ in range(q):
        s, k = input().split()
        k = int(k)

        found = None

        for i in range(len(s)):
            suf = s[i:]
            if suf in buckets:
                found = buckets[suf]
                break

        if not found or len(found) < k:
            print(-1)
        else:
            print(found[k - 1][1])

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý xây dựng một từ điển từ các chuỗi hậu tố đến tất cả các từ trong từ điển có chung hậu tố đó. Mỗi mục được lưu sẽ giữ cả từ và vị trí ban đầu của nó, vì việc sắp xếp được thực hiện theo từ nhưng đầu ra yêu cầu chỉ mục gốc. 

Mỗi nhóm được sắp xếp một lần để mọi truy vấn có thể truy cập trực tiếp vào phần tử từ điển thứ k mà không cần tính toán lại. 

Trong quá trình truy vấn, các hậu tố của từ truy vấn được kiểm tra từ dài nhất đến ngắn nhất. Tra cứu thành công đầu tiên đảm bảo độ dài kết hợp hậu tố tối đa. Nhóm tương ứng sau đó được sử dụng để trả lời yêu cầu xếp hạng. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán lại các kết quả khớp hậu tố một cách linh hoạt. Tất cả các so sánh được giảm xuống thành tra cứu từ điển trên các khóa được tính toán trước, giúp duy trì thời gian truy vấn không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một từ điển nhỏ và một bộ truy vấn. 

Từ điển:`["aba", "caba", "baba"]`Truy vấn:`"xxcaba", k = 2`| Bước | Đã kiểm tra hậu tố | Tồn tại trong Bản đồ | Nhóm đã chọn | 
| --- | --- | --- | --- | 
| 1 | "xxcaba" | Không | Không có | 
| 2 | "xcaba" | Không | Không có | 
| 3 | "caba" | Có | ["caba"] | 

Tại thời điểm này việc tìm kiếm dừng lại vì`"caba"`là hậu tố hợp lệ dài nhất. Nhóm chỉ chứa một từ nên k = 2 không hợp lệ và đầu ra là -1. Điều này chứng tỏ rằng các kết quả phù hợp với hậu tố ngắn hơn không bao giờ được xem xét khi tìm thấy kết quả dài hơn. 

Bây giờ hãy xem xét: 

Từ điển:`["abc", "xbc", "c", "bc"]`Truy vấn:`"zzbc", k = 2`| Bước | Đã kiểm tra hậu tố | Tồn tại trong Bản đồ | Nhóm đã chọn | 
| --- | --- | --- | --- | 
| 1 | "zzbc" | Không | Không có | 
| 2 | "zbc" | Không | Không có | 
| 3 | "bc" | Có | ["abc", "xbc", "bc"] | 

Sau khi sắp xếp theo từ điển, nhóm sẽ trở thành`["abc", "bc", "xbc"]`. Yếu tố thứ hai là`"bc"`, vì vậy đầu ra là chỉ mục ban đầu của nó. 

Dấu vết cho thấy việc lựa chọn hậu tố sẽ giảm vấn đề xuống một nhóm được tính toán trước như thế nào và thứ tự trong nhóm đó xác định câu trả lời cuối cùng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L + q · L + tổng sắp xếp) | Mỗi từ đóng góp tối đa 5 hậu tố, mỗi truy vấn kiểm tra tối đa 5 hậu tố và mỗi nhóm được sắp xếp một lần | 
| Không gian | O(n · L) | Mỗi hậu tố lưu trữ các tham chiếu đến các từ trong từ điển | 

Giá trị của L nhiều nhất là 5, do đó tất cả các yếu tố liên quan đến L đều hoạt động như chi phí không đổi. Điều này giúp cả thời gian và bộ nhớ thoải mái trong giới hạn ngay cả đối với 10^5 từ và truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample
assert run("""4
a
b
c
d
1
a 1
""") == "1"

# identical suffix competition
assert run("""3
aba
caba
baba
1
xxcaba 1
""") == "2"

# k too large
assert run("""3
abc
xbc
c
1
zzbc 5
""") == "-1"

# smallest inputs
assert run("""1
a
1
a 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| nhiều ứng cử viên có cùng hậu tố | chỉ số đúng | sắp xếp từ điển | 
| k vượt quá thùng | -1 | xử lý ranh giới | 
| lựa chọn hậu tố dài nhất | lọc đúng | quy tắc hậu tố tối đa | 

## Vỏ cạnh 

Trường hợp một truy vấn khớp với nhiều độ dài hậu tố được xử lý bằng cách quét các hậu tố từ dài nhất đến ngắn nhất. Lần tra cứu thành công đầu tiên luôn tương ứng với độ dài khớp tối đa có thể vì mọi hậu tố ngắn hơn chỉ được xem xét sau khi hết hậu tố dài hơn. 

Đối với một truy vấn như`"abcd"`với từ điển`"bcd"`Và`"cd"`, đầu tiên thuật toán sẽ kiểm tra`"abcd"`, sau đó`"bcd"`, và ngay lập tức dừng lại ở`"bcd"`. Hậu tố ngắn hơn`"cd"`không bao giờ được xem xét, điều này duy trì tính chính xác của yêu cầu “độ dài hậu tố tối đa”. 

Một trường hợp khác là khi nhiều từ có cùng hậu tố tốt nhất nhưng xuất hiện theo thứ tự đầu vào tùy ý. Vì mỗi nhóm được sắp xếp rõ ràng theo từ điển sau khi xây dựng nên việc lựa chọn phần tử thứ k độc lập với thứ tự đầu vào, ngăn chặn các câu trả lời sai do thứ tự chèn gây ra.
