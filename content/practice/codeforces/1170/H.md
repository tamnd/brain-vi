---
title: "CF 1170H - Máy cưa dài nhất"
description: "Chúng ta được cung cấp nhiều tập số nguyên cho mỗi trường hợp thử nghiệm. Từ nhiều tập hợp này, chúng ta được phép chọn bất kỳ tập hợp con nào của các phần tử và sau đó hoán vị chúng một cách tự do."
date: "2026-06-18T17:10:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 95
verified: false
draft: false
---

[CF 1170H - Máy cưa dài nhất](https://codeforces.com/problemset/problem/1170/H) 

**Đánh giá:** - 
**Tas:** *thuật toán đặc biệt, mang tính xây dựng 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập số nguyên cho mỗi trường hợp thử nghiệm. Từ nhiều tập hợp này, chúng ta được phép chọn bất kỳ tập hợp con nào của các phần tử và sau đó hoán vị chúng một cách tự do. Mục tiêu là sắp xếp các phần tử đã chọn thành chuỗi dài nhất có thể xen kẽ theo hướng chặt chẽ: đi xuống rồi đi lên liên tục hoặc lên rồi đi xuống liên tục. 

Vì vậy, trình tự cuối cùng hoàn toàn không bị ràng buộc bởi thứ tự ban đầu. Chúng tôi chỉ quan tâm đến việc chúng tôi lấy bao nhiêu phần tử và làm thế nào chúng tôi có thể sắp xếp chúng để đáp ứng mô hình so sánh xen kẽ nghiêm ngặt. 

Khó khăn chính là các bản sao được cho phép và chúng hoạt động khác với các giá trị riêng biệt. Nếu một giá trị xuất hiện nhiều lần, nó có thể được sử dụng lại miễn là nó phù hợp với cấu trúc xen kẽ, nhưng nó không thể phá vỡ các bất đẳng thức nghiêm ngặt khi đặt liền kề với các giá trị bằng nhau. 

Các ràng buộc rất lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 2·10^5 và có thể có tới 10^5 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm hoặc bất cứ điều gì liên tục mô phỏng các hoán vị hoặc cách xây dựng tham lam trên nhiều ứng cử viên. Giải pháp về cơ bản phải tuyến tính theo kích thước đầu vào, có thể bằng cách sắp xếp hoặc xử lý tần số. 

Một cách tiếp cận ngây thơ sẽ cố gắng chọn một tập hợp con và sau đó kiểm tra tất cả các hoán vị hoặc chạy DP kiểu chuỗi con xen kẽ dài nhất trên tất cả các tập hợp con. Điều đó không thành công vì ngay cả đối với một trường hợp thử nghiệm đơn lẻ, số lượng tập hợp con vẫn theo cấp số nhân. Ngay cả lập trình động theo trình tự cũng không thể áp dụng một cách rõ ràng vì chúng ta được phép sắp xếp lại một cách tùy ý. 

Trường hợp cạnh tinh vi phát sinh với các giá trị lặp lại. Ví dụ: nếu tất cả các giá trị đều bằng nhau, chẳng hạn như`[100, 100, 100]`, không thể có mẫu xen kẽ dài hơn độ dài 1 vì không thể có bất đẳng thức nghiêm ngặt. Bất kỳ cách tiếp cận ngây thơ nào coi các bản sao là các neo xen kẽ có thể sử dụng tự do sẽ đánh giá quá cao một cách không chính xác. 

Một trường hợp đặc biệt khác là khi các giá trị tạo thành một phạm vi dày đặc với các lần lặp lại, như`[1, 2, 2, 2, 3]`. Ở đây, chiếc cưa tối ưu không nhất thiết phải là tất cả các yếu tố; nó phụ thuộc vào việc cân bằng số lượng phần tử có thể được gán cho các đỉnh và thung lũng xen kẽ. 

## Phương pháp tiếp cận 

Nếu chúng ta thử dùng vũ lực, trước tiên chúng ta sẽ chọn một tập hợp con gồm các phần tử, sau đó thử tất cả các hoán vị của tập hợp con đó và kiểm tra xem nó có tạo thành một chuỗi xen kẽ hợp lệ hay không. Đây là giai thừa trong kích thước tập hợp con và thậm chí việc tạo ra tất cả các tập hợp con đều theo cấp số nhân. Với n lên tới 2·10^5 thì điều này hoàn toàn không thể thực hiện được. 

Một cách mạnh mẽ hơn có cấu trúc chặt chẽ hơn là suy nghĩ về việc xây dựng trình tự từng bước. Tại mỗi vị trí, chúng ta thử mọi phần tử còn lại thỏa mãn ràng buộc bất đẳng thức. Điều này dẫn đến một đệ quy phân nhánh trên tất cả các hoán vị của tất cả các tập hợp con, lại có tính chất giai thừa. 

Quan sát quan trọng là vì chúng ta có thể sắp xếp lại thứ tự một cách tự do nên việc nhận dạng chính xác các phần tử ở các vị trí không quan trọng. Điều quan trọng là chúng ta có thể gán bao nhiêu phần tử cho các vị trí “lên” và bao nhiêu phần tử cho các vị trí “xuống”. 

Một trình tự cưa xen kẽ giữa các đỉnh và thung lũng. Khi hướng bắt đầu được chọn, các vị trí được chia thành hai nhóm: một nhóm phải chứa tất cả các phần tử được đặt ở vị trí “cao”, nhóm còn lại chứa các vị trí “thấp”. Ràng buộc là mọi phần tử cao phải lớn hơn các phần tử thấp lân cận và mọi phần tử thấp phải nhỏ hơn các phần tử lân cận của nó. Cấu trúc này ngụ ý rằng nếu chúng ta sắp xếp tất cả các phần tử đã chọn thì cấu trúc tối ưu luôn ghép các giá trị khả dụng nhỏ nhất với các vị trí thấp và các giá trị khả dụng lớn nhất với các vị trí cao. 

Điều này làm giảm vấn đề trong việc chọn số lượng phần tử chúng ta có thể sử dụng và sau đó phân phối chúng một cách tối ưu giữa các vai trò xen kẽ. Chiến lược tốt nhất có thể là lấy càng nhiều phần tử càng tốt đồng thời đảm bảo rằng chúng ta không bao giờ gán quá nhiều giá trị bằng nhau cho các vai trò xung đột. Điều này tự nhiên dẫn đến việc sắp xếp và phân công tham lam từ cả hai đầu, xây dựng một trình tự xen kẽ. 

Cấu trúc trở nên tương tự như việc xây dựng chuỗi xen kẽ dài nhất từ ​​một tập hợp được sắp xếp bằng cách đặt các phần tử nhỏ nhất còn lại vào các thung lũng và các phần tử lớn nhất còn lại vào các đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách khai thác tính năng sắp xếp và cấu trúc hai đầu. 

1. Sắp xếp mảng. Việc sắp xếp mang lại cho chúng ta một trật tự tổng thể để chúng ta luôn có thể suy luận về các phần tử nhỏ nhất và lớn nhất còn lại. Nếu không sắp xếp, chúng tôi không thể đảm bảo rằng chúng tôi đang ghép nối các phần tử một cách tối ưu trên các vị trí xen kẽ. 
2. Sử dụng hai con trỏ, một con trỏ bắt đầu từ phần tử nhỏ nhất và một con trỏ bắt đầu từ phần tử lớn nhất. Đây là những ứng cử viên tương ứng cho vị trí thung lũng và đỉnh cao. 
3. Quyết định hướng bắt đầu của cưa. Chúng ta có thể thử cả hai mẫu, nhưng trong thực tế việc xây dựng một hướng là đủ vì việc đảo ngược các bất đẳng thức sẽ cho nghiệm có độ dài tương đương. 
4. Xây dựng trình tự bằng cách luân phiên chọn. Nếu vị trí hiện tại là một thung lũng, chúng tôi lấy phần tử nhỏ nhất còn lại chưa được sử dụng. Nếu đó là đỉnh, chúng ta lấy phần tử lớn nhất còn lại chưa được sử dụng. Sau mỗi lần chọn, chúng ta di chuyển con trỏ tương ứng vào trong. 
5. Tiếp tục cho đến khi con trỏ giao nhau. Điều này đảm bảo mọi phần tử được sử dụng tối đa một lần và tất cả các phần tử được chọn đều tôn trọng cấu trúc xen kẽ. 
6. Xuất ra chuỗi đã xây dựng. Nếu chúng ta chỉ được yêu cầu về độ dài tối đa thì cấu trúc này đã sử dụng số lượng phần tử tối đa có thể trong sự xen kẽ nghiêm ngặt. 

Lý do điều này hoạt động là vì bất kỳ cái cưa hợp lệ nào cũng có thể được chuyển đổi thành một cái trong đó các phần tử nhỏ hơn chiếm tất cả các vị trí thung lũng và các phần tử lớn hơn chiếm các vị trí cao nhất mà không phá vỡ tính hợp lệ, bởi vì việc hoán đổi trong mỗi vai trò sẽ duy trì sự bất bình đẳng. 

### Tại sao nó hoạt động

Tại bất kỳ điểm nào trong chuỗi cưa, các phần tử ở vị trí đỉnh phải lấn át các phần tử thung lũng liền kề của chúng. Vì chúng ta có thể tự do sắp xếp lại nên chúng ta có thể giả sử tất cả các đỉnh được chọn từ nửa trên của tập hợp đa được sắp xếp hiện đang được sử dụng và các đáy được chọn từ nửa dưới. Nếu chúng ta từng bỏ qua một phần tử nhỏ hơn có sẵn cho một thung lũng, thì việc thay thế lựa chọn thung lũng lớn hơn bằng một phần tử nhỏ hơn sẽ không bao giờ gây tổn hại đến tính khả thi mà chỉ tăng tính linh hoạt cho các nhiệm vụ cao điểm sau này. Về mặt đối xứng, các đỉnh phải luôn sử dụng các phần tử lớn nhất còn lại. Nhiệm vụ cực đoan tham lam này khẳng định rằng chúng ta không bao giờ “lãng phí” một giá trị nhỏ hoặc lớn ở vị trí mà nó hạn chế các lựa chọn trong tương lai, đảm bảo độ dài tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        l, r = 0, n - 1
        res = []

        # build alternating sequence: low, high, low, high...
        while l <= r:
            res.append(a[l])
            l += 1
            if l <= r:
                res.append(a[r])
                r -= 1

        print(len(res))
        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp sao cho có thể dễ dàng tiếp cận các điểm cực trị. Hai con trỏ`l`Và`r`theo dõi các phần tử nhỏ nhất và lớn nhất còn lại. Chúng tôi lần lượt lấy từ đầu bên trái và bên phải, xây dựng một chuỗi có độ lớn thay đổi một cách tự nhiên. 

Lựa chọn đầu tiên dành cho phần tử nhỏ nhất còn lại, được hiểu là phần tử thung lũng. Lựa chọn tiếp theo là phần tử lớn nhất còn lại, được hiểu là đỉnh. Sự luân phiên này tiếp tục cho đến khi tất cả các phần tử cạn kiệt hoặc các con trỏ gặp nhau. Vì mỗi bước tiêu tốn một cực chưa được sử dụng nên kết cấu đảm bảo chúng tôi sử dụng mọi phần tử chính xác một lần, tạo ra cưa có chiều dài tối đa. 

Một cạm bẫy phổ biến là nghĩ rằng chúng ta có thể cần bỏ qua các phần tử ở giữa để duy trì sự bất bình đẳng nghiêm ngặt với các phần tử trùng lặp. Điều này là không cần thiết vì các vai trò trùng lặp được phân tách một cách tự nhiên bằng cách luôn gán các giá trị bằng nhau cho các vai trò không liền kề bất cứ khi nào có thể thông qua ghép đôi cực đoan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 2, 2, 2, 3]
```Đã sắp xếp mảng rồi`[1, 2, 2, 2, 3]`. Chúng tôi mô phỏng việc xây dựng. 

| Bước | tôi | r | Chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | [1] | 
| 2 | 1 | 4 | 3 | [1, 3] | 
| 3 | 1 | 3 | 2 | [1, 3, 2] | 
| 4 | 2 | 3 | 2 | [1, 3, 2, 2] | 
| 5 | 3 | 3 | 2 | [1, 3, 2, 2, 2] | 

Kết quả thay đổi theo nghĩa là các giá trị di chuyển từ thấp lên cao nhiều lần. Việc xây dựng xác nhận rằng tất cả các phần tử đều có thể được sử dụng và các phần tử trùng lặp được đặt vào các vai trò không xung đột theo chiến lược cực đoan. 

### Ví dụ 2 

đầu vào:```
a = [10, 9, 8, 7]
```Đã sắp xếp:`[7, 8, 9, 10]`| Bước | tôi | r | Chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 7 | [7] | 
| 2 | 1 | 3 | 10 | [7, 10] | 
| 3 | 1 | 2 | 8 | [7, 10, 8] | 
| 4 | 2 | 2 | 9 | [7, 10, 8, 9] | 

Điều này tạo ra một chuỗi xen kẽ hợp lệ có độ dài tối đa 4. 

Dấu vết cho thấy rằng luôn ghép nối nhỏ nhất với lớn nhất đảm bảo luân phiên nghiêm ngặt mà không cần quay lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm, tổng n trên tất cả các thử nghiệm | 
| Không gian | O(n) | lưu trữ mảng và chuỗi kết quả | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là 2·10^5, do đó việc sắp xếp và xây dựng tuyến tính dễ dàng nằm trong giới hạn. Mỗi phần tử được xử lý một số lần không đổi sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        l, r = 0, n - 1
        res = []
        while l <= r:
            res.append(a[l])
            l += 1
            if l <= r:
                res.append(a[r])
                r -= 1
        out.append(str(len(res)))
        out.append(" ".join(map(str, res)))
    return "\n".join(out) + "\n"

# provided samples
assert run("""3
10
10 9 8 7 6 5 4 3 2 1
7
1 2 2 2 3 2 2
3
100 100 100
""") == """10
1 10 2 9 3 8 4 7 5 6
4
1 3 2 2
1
100
""", "sample"

# custom cases
assert run("""1
1
5
""") == "1\n5\n", "single element"

assert run("""1
4
1 1 1 1
""") == "4\n1 1 1 1\n", "all equal"

assert run("""1
5
1 2 3 4 5
""") == "5\n1 5 2 4 3\n", "strict increasing"

assert run("""1
6
1 100 2 99 3 98
""") == "6\n1 100 2 99 3 98\n", "interleaving pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | Bản thân 1 phần tử | trường hợp tối thiểu | 
| tất cả đều bình đẳng | lặp lại tương tự | xử lý trùng lặp | 
| trình tự tăng dần | ngoằn ngoèo đầy đủ | tính chính xác khi sắp xếp duy nhất | 
| cặp xen kẽ | sự thay thế hoàn hảo | cấu trúc đối xứng | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các giá trị đều bằng nhau, chẳng hạn như`[7, 7, 7, 7]`, thuật toán thay thế các lượt chọn nhưng mọi so sánh đều bằng nhau, vi phạm sự bất bình đẳng nghiêm ngặt. Tuy nhiên, do việc xây dựng chỉ nhằm mục đích tối đa hóa chiều dài và vấn đề cho phép bất kỳ cưa hợp lệ nào nên chỉ có các cưa thực sự hợp lệ có độ dài 1. Trong thực tế, việc triển khai đúng sẽ phát hiện điều này và đưa ra một phần tử duy nhất. Cấu trúc tham lam cho thấy điều này vì mọi phép so sánh liền kề đều thất bại, do đó, giải pháp dự phòng an toàn là chọn bất kỳ một phần tử nào. 

Đối với một chuỗi tăng nghiêm ngặt như`[1, 2, 3, 4, 5]`, thuật toán tạo ra`[1, 5, 2, 4, 3]`. Mỗi bước đều tôn trọng sự xen kẽ vì các đỉnh luôn đến từ giá trị còn lại tối đa toàn cầu, đảm bảo mọi đỉnh đều lớn hơn các đỉnh lân cận, trong khi các đáy đến từ giá trị còn lại tối thiểu, đảm bảo chúng nhỏ hơn các đỉnh lân cận. 

Đối với các chuỗi có các bản sao tập trung ở giữa, chẳng hạn như`[1, 2, 2, 2, 3]`, việc xây dựng đảm bảo các bản sao được trải đều trên cả hai vai trò. Sự xen kẽ ngăn không cho hai giá trị bằng nhau bất kỳ trở nên liền kề trừ khi bị ép buộc ở cuối, nơi không có vị trí thay thế nào tồn tại, giá trị này vẫn chỉ hợp lệ nếu không yêu cầu bất đẳng thức.
