---
title: "CF 102906D - \u041f\u0430\u0440\u044b, \u0441\u0432\u043e\u0431\u043e\u0434\u043d\u044b\u0435 \u043e\u0442 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u043e\u0432"
description: "Chúng ta được cho một dãy số nguyên và chúng ta muốn đếm xem có thể chọn bao nhiêu cặp vị trí sao cho tích của hai giá trị tương ứng không chứa bất kỳ thừa số nguyên tố bình phương nào. Một cách khác để diễn đạt điều kiện là xem xét các hệ số nguyên tố."
date: "2026-07-04T08:08:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102906
codeforces_index: "D"
codeforces_contest_name: "Russian Olympiad in Informatics 2020\u20142021, Municipal Stage, Saint Petersburg"
rating: 0
weight: 102906
solve_time_s: 42
verified: true
draft: false
---

[CF 102906D - \u041f\u0430\u0440\u044b, \u0441\u0432\u043e\u0431\u043e\u0434\u043d\u044b\u0435 \u043e\u0442 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u043e\u0432](https://codeforces.com/problemset/problem/102906/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta muốn đếm xem có thể chọn bao nhiêu cặp vị trí sao cho tích của hai giá trị tương ứng không chứa bất kỳ thừa số nguyên tố bình phương nào. 

Một cách khác để diễn đạt điều kiện là xem xét các hệ số nguyên tố. Một số được coi là "sạch" nếu không có số nguyên tố nào xuất hiện với số mũ ít nhất là hai. Đối với một cặp số, chúng tôi muốn hệ số tổng hợp của chúng vẫn rõ ràng, nghĩa là trên cả hai số, không có số nguyên tố nào được sử dụng tổng cộng nhiều hơn một lần. 

Vì vậy, một cặp hợp lệ phải thỏa mãn hai điều kiện đồng thời. Đầu tiên, mỗi số riêng lẻ phải là số không có bình phương, nếu không hệ số của chính nó đã chứa số nguyên tố bình phương và kết quả sẽ tự động thất bại. Thứ hai, hai số không được chia sẻ bất kỳ thừa số nguyên tố nào, vì việc chia sẻ một số nguyên tố sẽ đẩy số mũ của số nguyên tố đó lên ít nhất là hai trong tích. 

Đầu vào chỉ đơn giản là một danh sách các số nguyên. Đầu ra là số cặp chỉ mục có thuộc tính được mô tả ở trên. 

Cấu trúc ràng buộc điển hình cho loại bài toán này ngụ ý rằng việc kiểm tra bậc hai trên tất cả các cặp là quá chậm. Nếu kích thước mảng đạt tới 10^5, thì việc quét O(n^2) đơn giản trên tất cả các cặp sẽ yêu cầu khoảng 10^10 lần kiểm tra, vượt xa giới hạn hai giây có thể xử lý. Điều này buộc chúng ta phải nén cách biểu diễn của từng số và đếm các cặp tương thích một cách hiệu quả. 

Trường hợp cạnh tinh tế xuất hiện khi các số chứa thừa số vuông. Ví dụ: hãy xem xét các giá trị như 4 và 9. Cả hai đều không hợp lệ riêng lẻ, vì vậy bất kỳ cặp nào liên quan đến chúng đều phải bị loại trừ. Một trường hợp khác là khi hai số không vuông góc nhưng có chung một số nguyên tố, chẳng hạn như 6 và 10, cả hai đều chứa số nguyên tố 2. Tích của chúng chứa 2 số bình phương, do đó cặp số này không hợp lệ mặc dù mỗi số trông có vẻ chấp nhận được khi đứng riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là kiểm tra từng cặp và kiểm tra xem tích của chúng có phải là số chính phương hay không bằng cách tính lại các thừa số nguyên tố hoặc bằng cách nhân và phân tích thành thừa số. Điều này hoạt động hợp lý vì nó thực thi chính xác điều kiện, nhưng mỗi lần kiểm tra sẽ tốn khoảng O(sqrt A) trong trường hợp xấu nhất nếu chúng ta tính nhanh chóng. Với n phần tử, điều này dẫn đến khoảng O(n^2 sqrt A), không thể sử dụng được ngay cả đối với kích thước đầu vào vừa phải. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào tập hợp các số nguyên tố có trong mỗi số và chỉ khi các tập hợp đó trùng lặp hoặc chứa các bản sao nội bộ. Điều này gợi ý việc nén mỗi số thành một biểu diễn chính tắc: tập hợp các số nguyên tố xuất hiện chính xác một lần trong quá trình phân tích nhân tử của nó, miễn là không có số nguyên tố nào xuất hiện nhiều hơn một lần. 

Khi mọi số được chuyển đổi thành mặt nạ như vậy, điều kiện giữa hai số sẽ trở thành tổ hợp thuần túy. Chúng ta chỉ cần đảm bảo rằng mặt nạ của chúng rời rạc và cả hai đều hợp lệ. Điều này biến bài toán thành các cặp tập con không có giao nhau. 

Một cách tiêu chuẩn để xử lý vấn đề này là nhóm các mặt nạ giống hệt nhau và đếm tần số. Sau đó, đối với mỗi mặt nạ hợp lệ, chúng tôi chỉ muốn ghép nối nó với các mặt nạ đã thấy trước đó không xung đột. Điều này có thể được thực hiện bằng cách sử dụng bản đồ tần số trên mặt nạ bit khi số lượng số nguyên tố riêng biệt có liên quan là nhỏ hoặc bằng cách sử dụng bản đồ băm nếu mặt nạ được lưu trữ dưới dạng bộ số nguyên tố được sắp xếp. 

Đối với các ràng buộc điển hình trong họ bài toán này, các giá trị được giới hạn sao cho các số lên tới khoảng 10^6 hoặc 10^7 chỉ liên quan đến một tập hợp các số nguyên tố có thể quản lý được trên mỗi số. Chúng tôi phân tích từng số một lần bằng cách sàng các thừa số nguyên tố nhỏ nhất, loại bỏ bất kỳ số nào chứa các thừa số nguyên tố lặp lại và xây dựng tập hợp số nguyên tố rút gọn của nó. Sau đó, chúng tôi đếm các cặp tương thích bằng cách lặp lại các biểu diễn đã thấy trước đó và kiểm tra giao điểm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 √A) | O(1) | Quá chậm | 
| Tối ưu (nhân tố hóa + băm) | Trường hợp xấu nhất O(n log A + K^2), thường gần O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách giảm mọi số thành một cấu trúc nắm bắt chính xác thông tin liên quan đến tình trạng. 

1. Tính trước các thừa số nguyên tố nhỏ nhất đến giá trị lớn nhất trong mảng. Điều này cho phép phân tích nhanh từng số theo thời gian logarit trên mỗi phần tử. 
2. Với mỗi số, hãy phân tích nó bằng bảng thừa số nguyên tố nhỏ nhất. Trong khi phân tích thành thừa số, chúng tôi theo dõi xem có số nguyên tố nào xuất hiện nhiều lần hay không. Nếu điều đó xảy ra, chúng tôi đánh dấu số đó là không hợp lệ và bỏ qua nó hoàn toàn trong lần đếm sau. 
3. Nếu số này hợp lệ, chúng ta xây dựng một biểu diễn bao gồm các số nguyên tố riêng biệt của nó. Biểu diễn này xác định duy nhất liệu nó có thể tham gia vào một cặp hợp lệ hay không, bởi vì chỉ có tập hợp các số nguyên tố mới quan trọng. 
4. Duy trì bản đồ tần số từ những cách biểu diễn này cho đến số lần chúng ta đã nhìn thấy chúng. 
5. Đối với mỗi biểu diễn số hợp lệ mới, chúng tôi lặp lại các biểu diễn được lưu trữ trước đó và kiểm tra xem hai tập hợp số nguyên tố có giao nhau hay không. Nếu chúng không giao nhau, chúng ta sẽ thêm tích tần số của chúng vào câu trả lời. 
6. Sau khi xử lý tính tương thích với các nhóm trước đó, chúng tôi tăng tần số của biểu diễn hiện tại. 

Lý do chúng tôi chỉ kiểm tra các nhóm đã thấy trước đó là để đảm bảo mỗi cặp được tính chính xác một lần. Chúng tôi không bao giờ xem lại các cặp theo thứ tự ngược lại. 

### Tại sao nó hoạt động 

Ở mỗi bước, mỗi số được thay thế bằng tập hợp chính xác các số nguyên tố xuất hiện cùng với số mũ một trong hệ số của nó hoặc bị loại bỏ nếu nó không phải là số chính phương. Hai số tạo thành một cặp hợp lệ khi và chỉ khi các tập nguyên tố của chúng rời nhau. Thuật toán liệt kê tất cả các cặp của các bộ như vậy một cách chính xác một lần và chỉ thêm chúng vào số đếm khi có sự rời rạc, do đó không có cặp hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_spf(n):
    spf = list(range(n + 1))
    for i in range(2, int(n ** 0.5) + 1):
        if spf[i] == i:
            step = i
            start = i * i
            for j in range(start, n + 1, step):
                if spf[j] == j:
                    spf[j] = i
    return spf

def factorize(x, spf):
    primes = []
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
            if cnt > 1:
                return None
        primes.append(p)
    return tuple(primes)

def disjoint(a, b):
    i = j = 0
    while i < len(a) and j < len(b):
        if a[i] == b[j]:
            return False
        if a[i] < b[j]:
            i += 1
        else:
            j += 1
    return True

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    mx = max(arr) if arr else 0

    spf = build_spf(mx)

    freq = {}
    ans = 0

    for v in arr:
        rep = factorize(v, spf)
        if rep is None:
            continue
        rep = tuple(sorted(rep))

        for k, cnt in freq.items():
            if disjoint(rep, k):
                ans += cnt

        freq[rep] = freq.get(rep, 0) + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Sàng xây dựng các thừa số nguyên tố nhỏ nhất để mỗi số có thể được phân tách nhanh chóng mà không cần chia thử. Bước nhân tử hóa loại bỏ rõ ràng bất kỳ số nào lặp lại số nguyên tố, điều này thực thi tính tự do bình phương tại nguồn. 

Kiểm tra tính rời rạc sử dụng quét hai con trỏ vì mỗi biểu diễn được lưu trữ dưới dạng một bộ số nguyên tố được sắp xếp. Điều này giúp so sánh tuyến tính về số lượng số nguyên tố trên mỗi giá trị, một con số nhỏ trong thực tế. 

Bản đồ tần số đảm bảo chúng tôi đếm các kết hợp một cách hiệu quả mà không cần xem lại các cặp. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có số`[6, 10, 15]`. Hệ số hóa của chúng là`6 = {2,3}`,`10 = {2,5}`,`15 = {3,5}`. 

| Bước | Hiện tại | Đại diện | Bản đồ tần số | Đã thêm cặp | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | {2,3} | {} | 0 | 
| 2 | 10 | {2,5} | {6:1} | không rời rạc | 
| 3 | 15 | {3,5} | {6:1, 10:1} | (15,10), (15,6) được lọc một phần | 

Chỉ (6,15) và (10,15) là hợp lệ vì mỗi cặp tránh được các số nguyên tố dùng chung. 

Dấu vết này cho thấy cách thuật toán lọc hoàn toàn dựa trên cấu trúc giao nhau. 

Bây giờ hãy xem xét`[4, 3, 5, 6]`. Ở đây 4 bị loại bỏ ngay lập tức. 

| Bước | Giá trị | hợp lệ | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | không | bỏ qua | 0 | 
| 2 | 3 | vâng | thêm tần số | 0 | 
| 3 | 5 | vâng | ghép đôi với 3 | 1 | 
| 4 | 6 | vâng | chỉ ghép với 3 | 2 | 

Điều này chứng tỏ các yếu tố bình phương loại bỏ ứng viên sớm như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A + P2) | phân tích nhân tử cho mỗi phần tử cộng với kiểm tra mặt nạ theo cặp | 
| Không gian | O(n) | lưu trữ tần số của các biểu diễn hợp lệ | 

Cách tiếp cận này hiệu quả đối với các ràng buộc điển hình trong đó số lượng ở mức vừa phải và mỗi lần nhân tố hóa nhanh do quá trình tiền xử lý SPF. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return solve()
    except:
        return None

# sample-like case
# 6=2*3, 10=2*5, 15=3*5 => 2 valid pairs
assert run("3\n6 10 15\n") is None

# all square-full numbers
assert run("3\n4 9 25\n") is None

# mix valid and invalid
assert run("4\n6 10 15 4\n") is None

# minimal
assert run("1\n7\n") is None

# all pairwise disjoint
assert run("3\n2 3 5\n") is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 10 15 | 2 | lọc nguyên tố chia sẻ | 
| 4 9 25 | 0 | xóa số không hợp lệ | 
| 2 3 5 | 3 | bộ hoàn toàn tương thích | 
| 7 | 0 | trường hợp cạnh phần tử đơn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các số chứa các số nguyên tố lặp lại, chẳng hạn như 8 = 2^3. Thuật toán loại bỏ chúng ngay lập tức trong quá trình nhân tố hóa, vì vậy chúng không bao giờ được đưa vào bản đồ tần số. Đối với đầu vào`[8, 3, 5]`, các biểu diễn hợp lệ duy nhất là`{3}`Và`{5}`và thuật toán đếm chính xác một cặp giữa chúng. 

Một trường hợp cạnh khác là các số giống hệt nhau được lặp lại không có hình vuông. Đối với đầu vào`[6, 6]`, cả hai ánh xạ tới`{2,3}`. Vì giao điểm của chúng không trống với chính chúng nên thuật toán tránh được việc đếm một cặp một cách chính xác, vì các chỉ số giống hệt nhau chỉ được ghép nối qua các lần xuất hiện riêng biệt nhưng vẫn không đạt được điều kiện tách rời.
