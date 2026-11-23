---
title: "CF 45I - TCMCF+++"
description: "Chúng ta được cung cấp một danh sách các số nguyên biểu thị điểm của bài toán trong một cuộc thi. Một thí sinh có thể giải bất kỳ tập con không trống nào của các bài toán này và điểm cuối cùng sẽ trở thành tích của tất cả các giá trị đã chọn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "I"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 1400
weight: 45
solve_time_s: 138
verified: false
draft: false
---

[CF 45I - TCMCF+++](https://codeforces.com/problemset/problem/45/I) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên biểu thị điểm của bài toán trong một cuộc thi. Một thí sinh có thể giải bất kỳ tập con không trống nào của các bài toán này và điểm cuối cùng sẽ trở thành tích của tất cả các giá trị đã chọn. 

Nhiệm vụ không phải là tính toán tích cực đại mà là xuất ra một tập hợp con có tích lớn nhất có thể. 

Những hạn chế là rất nhỏ. Có nhiều nhất 100 số, mỗi số nằm trong khoảng từ -100 đến 100. Mặc dù giới hạn rất nhỏ nhưng số lượng tập hợp con vẫn theo cấp số nhân. Tìm kiếm brute-force sẽ yêu cầu kiểm tra tất cả$2^n - 1$tập hợp con không trống. Với$n = 100$, điều đó là hoàn toàn không thể. 

Phần khó khăn của vấn đề là xử lý chính xác các số âm và số 0. Các giá trị dương luôn có ích vì nhân với số dương lớn hơn 0 sẽ làm tăng tích. Các giá trị âm tinh tế hơn. Số âm chẵn tạo ra sản phẩm dương, trong khi số lẻ tạo ra sản phẩm âm. 

Một số trường hợp phức tạp dễ dàng phá vỡ các giải pháp bất cẩn. 

Giả sử tất cả các số đều âm và số của chúng là số lẻ. 

đầu vào:```
3
-1 -2 -3
```Việc chọn tất cả các số sẽ mang lại$(-1) \cdot (-2) \cdot (-3) = -6$. 

Nhưng chỉ chọn`-2 -3`cho`6`, cái nào lớn hơn. 

Câu trả lời đúng sẽ loại bỏ số âm có giá trị tuyệt đối nhỏ nhất ở đây`-1`. 

Bây giờ hãy xem xét trường hợp chỉ có một số âm và số không. 

đầu vào:```
4
-5 0 0 0
```Lựa chọn`-5`cho`-5`. 

Việc chọn bất kỳ số 0 nào sẽ mang lại`0`, cái nào lớn hơn. 

Cách tiếp cận tham lam luôn giữ các giá trị khác 0 sẽ thất bại ở đây. 

Một tình huống nguy hiểm khác là khi mọi giá trị đều bằng 0. 

đầu vào:```
3
0 0 0
```Chúng ta vẫn phải xuất ra ít nhất một số. 

Sản phẩm tốt nhất có thể là`0`, vì vậy việc in một số 0 là chính xác. 

Đầu vào hợp lệ nhỏ nhất cũng có vấn đề. 

đầu vào:```
1
-7
```Chúng tôi buộc phải lấy số duy nhất có sẵn, mặc dù tích số âm. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Chúng tôi lặp lại mọi tập hợp con không trống, tính tích của nó và giữ lại tập hợp con có kết quả lớn nhất. 

Điều này hiệu quả vì định nghĩa của điểm trực tiếp phụ thuộc vào các tập hợp con. Mỗi ứng viên có thể được đánh giá độc lập. 

Vấn đề là số lượng tập hợp con. Với$n = 100$, tổng số trở thành xấp xỉ$1.27 \times 10^{30}$. Ngay cả việc kiểm tra một tỷ tập hợp con mỗi giây cũng không thể hoàn thành trong vòng đời của vũ trụ. 

Quan sát quan trọng là phép nhân hoạt động rất dễ đoán với các dấu hiệu. 

Những con số dương luôn hữu ích. Nhân với một số dương lớn hơn 1 sẽ làm tăng tích. Nhân với 1 giữ cho kết quả không thay đổi nhưng vẫn không bao giờ bị tổn hại. 

Số âm chỉ hữu ích khi đi theo cặp. Hai tiêu cực tạo nên sự đóng góp tích cực. Nếu số âm bản là số lẻ thì phải loại bỏ chính xác một âm bản. Để giảm độ lớn càng ít càng tốt, chúng ta nên loại bỏ số âm gần nhất bằng 0. 

Zeros không bao giờ cải thiện một sản phẩm tích cực. Mục đích duy nhất của họ là giải quyết các tình huống trong đó mọi sản phẩm khác 0 có thể đạt được sẽ là số âm. 

Điều này biến vấn đề thành một quá trình lựa chọn tham lam đơn giản. 

Chúng tôi giữ: 

1. Mọi số dương. 
2. Mọi số âm có thể ngoại trừ một. 
3. Nếu số âm là số lẻ, hãy loại bỏ số âm có giá trị tuyệt đối nhỏ nhất. 
4. Nếu không còn gì, xuất ra một số 0 nếu có, nếu không thì xuất ra số âm duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số và tách chúng thành số dương, số âm và số không. 

Điều này làm cho các quyết định sau này trở nên đơn giản hơn nhiều vì mỗi danh mục hoạt động khác nhau khi nhân. 
2. Thêm mọi số dương vào câu trả lời. 

Bất kỳ giá trị dương nào cũng giúp ích hoặc ít nhất là không làm giảm sản phẩm. 
3. Sắp xếp các số âm theo giá trị tuyệt đối. 

Số âm gần nhất với 0 là số ít hữu ích nhất khi chúng ta cần loại bỏ số âm. 
4. Nếu số câu phủ định là số chẵn, hãy cộng tất cả các câu phủ định vào câu trả lời. 

Sản phẩm của họ trở nên tích cực. 
5. Nếu số âm là số lẻ, hãy loại bỏ chính xác một âm, cụ thể là giá trị âm có giá trị tuyệt đối nhỏ nhất và cộng phần còn lại. 

Việc loại bỏ bất kỳ tiêu cực nào khác sẽ làm giảm cường độ cuối cùng một cách nghiêm trọng hơn. 
6. Nếu đáp án vẫn trống thì xử lý các trường hợp đặc biệt. 

Nếu số 0 tồn tại, xuất ra một số 0 vì$0$lớn hơn bất kỳ tích âm nào. 

Nếu không, có chính xác một số âm trong toàn bộ mảng, vì vậy hãy xuất nó ra. 
7. In các số đã chọn. 

### Tại sao nó hoạt động 

Thuật toán tối đa hóa dấu đầu tiên, sau đó là độ lớn. 

Một tích dương luôn tốt hơn 0 và số 0 luôn tốt hơn một tích âm. Trong số các tích dương, giá trị tuyệt đối càng lớn thì càng tốt. 

Nên đưa vào tất cả các số dương vì việc nhân với chúng không bao giờ làm giảm tích. Các số âm chỉ đóng góp tích cực theo cặp, do đó, số lẻ buộc phải loại bỏ một. Việc loại bỏ số âm gần nhất bằng 0 sẽ giữ nguyên giá trị tuyệt đối lớn nhất có thể cho sản phẩm còn lại. 

Nếu không thể tạo ra sản phẩm dương thì chọn số 0 là tối ưu bất cứ khi nào có thể. Chỉ khi mảng chứa một số âm duy nhất và không có số 0 thì chúng ta buộc phải xuất ra một giá trị âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    positives = []
    negatives = []
    zeros = []

    for x in a:
        if x > 0:
            positives.append(x)
        elif x < 0:
            negatives.append(x)
        else:
            zeros.append(x)

    negatives.sort(key=abs)

    ans = []

    ans.extend(positives)

    if len(negatives) % 2 == 0:
        ans.extend(negatives)
    else:
        ans.extend(negatives[1:])

    if not ans:
        if zeros:
            ans.append(0)
        else:
            ans.append(negatives[0])

    print(*ans)

solve()
```Giải pháp bắt đầu bằng cách chia các số thành ba nhóm vì số dương, số âm và số 0 yêu cầu các chiến lược xử lý khác nhau. 

Các số âm được sắp xếp theo giá trị tuyệt đối thay vì giá trị số. Chi tiết này quan trọng. Chúng tôi muốn xóa số âm gần nhất với 0 chứ không phải giá trị số nhỏ nhất. Ví dụ, giữa`-100`Và`-1`, loại bỏ`-1`tốt hơn nhiều. 

Dòng:```
ans.extend(negatives[1:])
```hoạt động vì các số âm được sắp xếp theo giá trị tuyệt đối. Phần tử đầu tiên là phần tử phủ định ít hữu ích nhất, vì vậy việc bỏ qua phần tử này sẽ để lại tập hợp có kích thước chẵn tối ưu. 

Khối trường hợp đặc biệt cuối cùng là cần thiết. Không có nó, đầu vào như:```
3
0 0 0
```sẽ tạo ra một đầu ra trống, điều này không hợp lệ vì phải giải quyết ít nhất một vấn đề. 

Số nguyên Python tự động xử lý các sản phẩm lớn nên việc tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 -3 3 3
```| Bước | Tích cực | Tiêu cực | Số không | Trả lời | 
| --- | --- | --- | --- | --- | 
| Chia ban đầu |`[1,2,3,3]`|`[-3]`|`[]`|`[]`| 
| Thêm mặt tích cực |`[1,2,3,3]`|`[-3]`|`[]`|`[1,2,3,3]`| 
| Những tiêu cực kỳ lạ, loại bỏ`-3`|`[1,2,3,3]`|`[-3]`|`[]`|`[1,2,3,3]`| 

Đầu ra:```
1 2 3 3
```Dấu vết cho thấy rằng một âm bản đơn độc sẽ có hại vì nó biến toàn bộ sản phẩm thành âm bản. 

### Ví dụ 2 

đầu vào:```
4
-5 0 0 0
```| Bước | Tích cực | Tiêu cực | Số không | Trả lời | 
| --- | --- | --- | --- | --- | 
| Chia ban đầu |`[]`|`[-5]`|`[0,0,0]`|`[]`| 
| Những tiêu cực kỳ lạ, loại bỏ`-5`|`[]`|`[-5]`|`[0,0,0]`|`[]`| 
| Câu trả lời trống, chọn số 0 |`[]`|`[-5]`|`[0,0,0]`|`[0]`| 

Đầu ra:```
0
```Ví dụ này chứng minh tại sao số 0 lại quan trọng. Một sản phẩm bằng 0 tốt hơn một sản phẩm âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp số âm chiếm ưu thế | 
| Không gian |$O(n)$| Mảng câu trả lời và danh mục lưu trữ các giá trị đầu vào | 

Với$n \le 100$, giải pháp này dễ dàng đủ nhanh. Thậm chí một$O(n^2)$lời giải sẽ trôi qua một cách thoải mái, do đó chi phí sắp xếp logarit là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    positives = []
    negatives = []
    zeros = []

    for x in a:
        if x > 0:
            positives.append(x)
        elif x < 0:
            negatives.append(x)
        else:
            zeros.append(x)

    negatives.sort(key=abs)

    ans = []

    ans.extend(positives)

    if len(negatives) % 2 == 0:
        ans.extend(negatives)
    else:
        ans.extend(negatives[1:])

    if not ans:
        if zeros:
            ans.append(0)
        else:
            ans.append(negatives[0])

    print(*ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
res = run("5\n1 2 -3 3 3\n")
assert set(map(int, res.split())) == {1, 2, 3}

# minimum size
assert run("1\n-7\n") == "-7", "single negative"

# all zeros
assert run("3\n0 0 0\n") == "0", "all zeros"

# odd number of negatives
res = run("3\n-1 -2 -3\n")
vals = sorted(map(int, res.split()))
assert vals == [-3, -2], "remove smallest abs negative"

# even number of negatives
res = run("4\n-1 -2 -3 -4\n")
vals = sorted(map(int, res.split()))
assert vals == [-4, -3, -2, -1], "keep all negatives"

# positives and zeros
res = run("5\n0 1 2 0 3\n")
vals = sorted(map(int, res.split()))
assert vals == [1, 2, 3], "ignore zeros"

# large boundary style case
arr = " ".join(["-1"] * 99 + ["2"])
res = run(f"100\n{arr}\n")
vals = list(map(int, res.split()))
assert len(vals) == 99, "discard one negative from odd count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -7`|`-7`| Đầu vào kích thước tối thiểu | 
|`0 0 0`|`0`| Xử lý câu trả lời trống | 
|`-1 -2 -3`|`-2 -3`| Những tiêu cực kỳ lạ yêu cầu loại bỏ một lần | 
|`-1 -2 -3 -4`| cả bốn tiêu cực | Ngay cả những tiêu cực cũng nên giữ nguyên | 
|`0 1 2 0 3`|`1 2 3`| Nên bỏ qua số không khi có mặt tích cực | 
| 99 âm bản và một tích cực | 99 giá trị được chọn | Hành vi ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3
-1 -2 -3
```Các số âm được sắp xếp theo giá trị tuyệt đối trở thành:```
[-1, -2, -3]
```Có ba số âm nên số đếm là số lẻ. Thuật toán loại bỏ`-1`vì giá trị tuyệt đối của nó nhỏ nhất. Sản phẩm còn lại trở thành:```
(-2) * (-3) = 6
```Bất kỳ sự loại bỏ nào khác đều tạo ra một sản phẩm nhỏ hơn. 

Bây giờ hãy xem xét:```
4
-5 0 0 0
```Thuật toán ban đầu loại bỏ`-5`bởi vì nó là số âm duy nhất và số đếm là số lẻ. Câu trả lời trở nên trống rỗng. Vì số 0 tồn tại nên thuật toán đưa ra`0`. 

Điều này đúng vì:```
0 > -5
```Cuối cùng, kiểm tra trường hợp hoàn toàn bằng 0:```
3
0 0 0
```Không có tích cực hoặc tiêu cực được chọn. Câu trả lời vẫn trống cho đến khi xử lý trường hợp đặc biệt thêm một số 0. 

Nếu không có bước này, chương trình sẽ không in sai gì cả, vi phạm yêu cầu phải chọn ít nhất một bài toán.
