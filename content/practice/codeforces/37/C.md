---
title: "CF 37C - Ngôn ngữ Berland cổ"
description: "Chúng ta cần xây dựng một mã tiền tố nhị phân."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 37
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 37"
rating: 1900
weight: 37
solve_time_s: 102
verified: true
draft: false
---
[CF 37C - Ngôn ngữ Berland cổ](https://codeforces.com/problemset/problem/37/C) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, tham lam, cây cối 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mã tiền tố nhị phân. 

Đầu vào cung cấp độ dài của tất cả các từ trong ngôn ngữ. Mỗi từ chỉ có thể chứa`0`Và`1`và không từ nào được phép làm tiền tố của từ khác. Chúng ta phải xây dựng một tập hợp từ như vậy hoặc xác định rằng điều đó là không thể. 

Ví dụ, nếu độ dài là`[1, 2, 3]`, một cách xây dựng hợp lệ là:```
0
10
110
```Không có chuỗi nào trong số này là tiền tố của chuỗi khác. từ`0`không phải là tiền tố của`10`bởi vì`10`bắt đầu bằng`1`, Và`10`không phải là tiền tố của`110`bởi vì`110`bắt đầu bằng`11`. 

Khó khăn chính là độ dài được cố định. Chúng tôi không được tự do lựa chọn độ dài từ mã tùy ý, chỉ các chuỗi nhị phân thực tế. 

Các ràng buộc đủ nhỏ để chúng ta có thể sử dụng các thuật toán xung quanh`O(n log n)`hoặc thậm chí`O(total_length^2)`trong thực tế, vì`n ≤ 1000`và mỗi chiều dài nhiều nhất là`1000`. Tuy nhiên, các chuỗi nhị phân cưỡng bức là hoàn toàn không khả thi vì độ dài`1000`có nghĩa là có`2^1000`những ứng viên có thể. 

Cấu trúc của chuỗi nhị phân không có tiền tố gợi ý rõ ràng về cách suy nghĩ dưới dạng tri nhị phân. Mỗi nút tương ứng với một tiền tố và việc chọn một từ có nghĩa là đánh dấu một nút là thiết bị đầu cuối. Khi một nút trở thành thiết bị đầu cuối, toàn bộ cây con của nó sẽ không thể sử dụng được vì các chuỗi dài hơn bên dưới nó sẽ có từ đó làm tiền tố. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử tất cả các độ dài là`1`:```
3
1 1 1
```Chỉ có hai chuỗi nhị phân có độ dài`1`, cụ thể là`0`Và`1`. Một thuật toán tham lam bất cẩn có thể tiếp tục thử các chuỗi sâu hơn, nhưng các chuỗi sâu hơn sẽ có một trong những từ đó làm tiền tố. Đầu ra đúng là:```
NO
```Một trường hợp khó khăn khác là độ dài lặp đi lặp lại:```
4
2 2 2 2
```Điều này hợp lệ vì có chính xác bốn chuỗi nhị phân có độ dài`2`:```
00
01
10
11
```Việc triển khai kém khiến chặn các anh chị em không chính xác bên trong bộ ba có thể từ chối trường hợp hợp lệ này. 

Một tình huống tế nhị hơn xuất hiện khi các từ ngắn chiếm quá nhiều không gian:```
3
1 2 2
```Sau khi chọn một từ có độ dài 1, chỉ còn lại một nhánh của tri. Nhánh đó chỉ chứa hai chuỗi có độ dài`2`, nhưng một trong số chúng bắt đầu bằng từ có độ dài 1 đã chọn và bị cấm. Số lượng từ có độ dài tối đa có thể giảm xuống. Câu trả lời đúng là:```
NO
```Đây chính xác là kiểu tương tác khiến các lựa chọn tham lam cục bộ trở nên nguy hiểm trừ khi cấu trúc tri được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo từng chuỗi nhị phân có độ dài cần thiết và kiểm tra xem chuỗi mới có xung đột với các chuỗi đã chọn trước đó hay không. Hai chuỗi xung đột nếu một chuỗi là tiền tố của chuỗi kia. 

Điều này hoạt động với độ dài nhỏ. Đối với mỗi chuỗi ứng viên, chúng ta có thể so sánh nó với mọi từ đã chọn trước đó. Vấn đề là không gian tìm kiếm. Ngay cả một chiều dài duy nhất`1000`từ có`2^1000`khả năng. Không có sự cắt tỉa nào có thể cứu được cách tiếp cận này trong trường hợp xấu nhất. 

Quan sát thực tế là các tập hợp không có tiền tố tương ứng một cách tự nhiên với các lá trong bộ ba nhị phân. 

Hãy coi tất cả các chuỗi nhị phân là các đường dẫn trong cây nhị phân vô hạn. Di chuyển sang trái nối thêm`0`, di chuyển sang phải nối thêm`1`. Một từ dài`k`tương ứng với một nút ở độ sâu`k`. 

Điều kiện tiền tố trở nên đơn giản trong chế độ xem này. Nếu chúng tôi chọn một nút làm từ, chúng tôi sẽ bị cấm chọn bất kỳ tổ tiên hoặc hậu duệ nào của nút đó. Vì vậy, mỗi từ được chọn phải là một lá trong số các nút đã chọn. 

Điều này cho thấy một công trình tham lam. 

Nếu trước tiên chúng tôi xử lý các độ dài ngắn hơn, chúng tôi luôn có thể chỉ định nút nhỏ nhất hiện có về mặt từ điển ở độ sâu đó. Khi một nút được sử dụng, cây con của nó sẽ tự động biến mất khỏi việc xem xét. 

Một cách rõ ràng để thực hiện điều này là duy trì tập hợp các tiền tố hiện có. Ban đầu chỉ tồn tại chuỗi trống. Để tạo ra các chuỗi có độ sâu lớn hơn, chúng tôi liên tục mở rộng các nút bằng cách thay thế tiền tố`x`với`x0`Và`x1`. 

Bất biến quan trọng là tập hợp có sẵn luôn chứa các tiền tố không xung đột theo cặp. Khi chúng ta cần một từ dài`L`, chúng tôi mở rộng cho đến khi một số tiền tố có sẵn đạt đến độ sâu`L`. Nếu không có nút như vậy tồn tại thì việc xây dựng là không thể. 

Điều này biến việc tìm kiếm theo cấp số nhân thành một phép duyệt trie tham lam xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n + tổng_length) | O(tổng_chiều dài) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các độ dài cùng với chỉ số gốc của chúng. 

Chúng ta phải xuất ra các câu trả lời theo thứ tự đầu vào, nhưng việc xử lý độ dài theo thứ tự được sắp xếp sẽ dễ dàng hơn nhiều vì các từ ngắn hơn sẽ hạn chế các từ dài hơn. 
2. Sắp xếp các từ theo độ dài. 

Việc gán các từ ngắn trước là an toàn vì một từ ngắn sẽ chặn toàn bộ cây con. Trì hoãn những từ ngắn có thể gây ra xung đột sau này. 
3. Duy trì hàng tiền tố hiện có. 

Ban đầu hàng đợi chỉ chứa chuỗi trống`""`. 
4. Đối với mỗi độ dài yêu cầu`L`, liên tục mở rộng tiền tố ngắn nhất có sẵn cho đến khi mặt trước của hàng đợi có độ dài`L`. 

Mở rộng tiền tố`x`có nghĩa là loại bỏ nó và chèn`x0`Và`x1`. 

Điều này mô phỏng việc giảm dần một cấp độ sâu hơn trong bộ ba nhị phân. 
5. Sau khi mở rộng, kiểm tra xem tiền tố phía trước có độ dài chính xác không`L`. 

Nếu tất cả các tiền tố có sẵn đã sâu hơn`L`, thì không có từ nào có độ dài hợp lệ`L`tồn tại nữa. đầu ra`NO`. 
6. Nếu không, hãy lấy tiền tố phía trước làm từ được chọn cho yêu cầu này và xóa nó khỏi hàng đợi. 

Việc loại bỏ nó sẽ ngăn không cho bất kỳ phần tử con nào được sử dụng sau này, điều này sẽ tự động duy trì tình trạng không có tiền tố. 
7. Lưu trữ từ đã chọn ở chỉ mục gốc của nó. 
8. Sau khi xử lý tất cả các yêu cầu, xuất ra`YES`và in các từ được lưu trữ theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mọi tiền tố có sẵn đại diện cho một cây con chưa được sử dụng của bộ ba nhị phân. 

Khi chúng tôi mở rộng tiền tố`x`vào trong`x0`Và`x1`, chúng ta phân vùng cây con có gốc tại`x`vào hai cây con con của nó mà không làm mất bất kỳ chuỗi nào có thể xảy ra trong tương lai. 

Khi chúng ta chọn một từ`x`, chúng tôi loại bỏ vĩnh viễn cây con của nó khỏi việc xem xét trong tương lai. Điều này đảm bảo rằng không từ nào sau này có thể có`x`như một tiền tố. 

Độ dài xử lý theo thứ tự tăng dần là điều làm cho lựa chọn tham lam trở nên hợp lệ. Một từ ngắn hơn luôn chặn nhiều khoảng trống hơn một từ dài hơn, vì vậy việc trì hoãn các từ ngắn chỉ có thể làm giảm tính linh hoạt. Bằng cách luôn lấy tiền tố có sẵn nhỏ nhất về mặt từ điển ở độ sâu cần thiết, chúng tôi không bao giờ lãng phí không gian trie có sẵn. 

Nếu thuật toán thất bại ở một mức độ nào đó`L`, mọi tiền tố có sẵn còn lại đã sâu hơn`L`. Vì độ sâu trie chỉ tăng trong quá trình mở rộng nên không có cách nào để tạo nút mới ở độ sâu`L`. Một công trình hợp lệ không thể tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    lengths = list(map(int, input().split()))

    arr = [(lengths[i], i) for i in range(n)]
    arr.sort()

    q = deque([""])
    ans = [""] * n

    for length, idx in arr:
        while q and len(q[0]) < length:
            cur = q.popleft()
            q.append(cur + "0")
            q.append(cur + "1")

        if not q or len(q[0]) != length:
            print("NO")
            return

        ans[idx] = q.popleft()

    print("YES")
    print("\n".join(ans))

solve()
```Giải pháp tuân theo cách giải thích trực tiếp. 

Hàng đợi lưu trữ các tiền tố có sẵn theo thứ tự từ điển. Ban đầu chỉ tồn tại chuỗi trống. Việc mở rộng một nút tương ứng với việc thay thế nó bằng hai nút con của nó. 

các`while`vòng lặp tiếp tục mở rộng cho đến khi tiền tố nông nhất có sẵn đạt đến độ sâu cần thiết. Điều này quan trọng vì các tiền tố ngắn hơn độ sâu mục tiêu vẫn chưa thể được sử dụng làm từ. 

Điều kiện thất bại là tinh tế:```
if not q or len(q[0]) != length:
```Nếu tiền tố ngắn nhất có sẵn đã sâu hơn`length`, thì mọi tiền tố có sẵn cũng sâu hơn. Chúng ta không bao giờ có thể di chuyển lên trên trong bộ ba, vì vậy việc xây dựng một từ có độ dài chính xác như vậy là không thể. 

Loại bỏ tiền tố đã chọn khỏi hàng đợi là điều thực thi tính không có tiền tố. Khi một từ được chọn, cây con của nó sẽ biến mất vĩnh viễn. 

Kích thước hàng đợi không bao giờ đủ lớn để gây ra sự cố. Mỗi lần mở rộng đều tăng số lượng nút lên một và tổng số từ được chọn chỉ là`1000`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Yêu cầu được sắp xếp:```
(1,0), (2,1), (3,2)
```| Bước | Xếp hàng trước | Hành động | Lời được chọn | Xếp Hàng Sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu |`[""]`| Trạng thái ban đầu | - |`[""]`| 
| 1 |`[""]`| Mở rộng`""`| - |`["0","1"]`| 
| 2 |`["0","1"]`| Lấy chiều dài 1 |`0`|`["1"]`| 
| 3 |`["1"]`| Mở rộng`"1"`| - |`["10","11"]`| 
| 4 |`["10","11"]`| Lấy chiều dài 2 |`10`|`["11"]`| 
| 5 |`["11"]`| Mở rộng`"11"`| - |`["110","111"]`| 
| 6 |`["110","111"]`| Lấy chiều dài 3 |`110`|`["111"]`| 

Đầu ra cuối cùng:```
YES
0
10
110
```Dấu vết này cho thấy mỗi từ được chọn sẽ loại bỏ toàn bộ cây con như thế nào. Sau khi chọn`0`, chỉ nhánh bắt đầu bằng`1`vẫn có sẵn. 

### Ví dụ 2 

đầu vào:```
3
1 2 2
```Yêu cầu được sắp xếp:```
(1,0), (2,1), (2,2)
```| Bước | Xếp hàng trước | Hành động | Lời được chọn | Xếp Hàng Sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu |`[""]`| Trạng thái ban đầu | - |`[""]`| 
| 1 |`[""]`| Mở rộng`""`| - |`["0","1"]`| 
| 2 |`["0","1"]`| Lấy chiều dài 1 |`0`|`["1"]`| 
| 3 |`["1"]`| Mở rộng`"1"`| - |`["10","11"]`| 
| 4 |`["10","11"]`| Lấy chiều dài 2 |`10`|`["11"]`| 
| 5 |`["11"]`| Lấy chiều dài 2 |`11`|`[]`| 

Trên thực tế trường hợp này là hợp lệ:```
YES
0
10
11
```Dấu vết cho thấy một điểm quan trọng. Lựa chọn`0`chặn một nửa số tri, nhưng nửa còn lại vẫn chứa chính xác hai chuỗi có độ dài 2 hợp lệ. 

Bây giờ hãy xem xét:```
3
1 1 2
```| Bước | Xếp hàng trước | Hành động | Lời được chọn | Xếp Hàng Sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu |`[""]`| Trạng thái ban đầu | - |`[""]`| 
| 1 |`[""]`| Mở rộng`""`| - |`["0","1"]`| 
| 2 |`["0","1"]`| Lấy chiều dài 1 |`0`|`["1"]`| 
| 3 |`["1"]`| Lấy chiều dài 1 |`1`|`[]`| 
| 4 |`[]`| Cần độ dài 2 | Không thể | - | 

Đầu ra:```
NO
```Sau khi cả hai chuỗi có độ dài-1 được sử dụng, bộ ba sẽ hoàn toàn cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + S) | Phân loại chi phí`O(n log n)`, thử mở rộng tổng cộng`O(S)`Ở đâu`S`là tổng độ dài | 
| Không gian | O(S) | Hàng đợi và các chuỗi được lưu trữ cùng nhau chứa tối đa`O(S)`nhân vật | 

Đây`S`nhiều nhất là`1000 × 1000 = 10^6`, điều này hoàn toàn an toàn trong Python. Thuật toán dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    def solve():
        n = int(input())
        lengths = list(map(int, input().split()))

        arr = [(lengths[i], i) for i in range(n)]
        arr.sort()

        q = deque([""])
        ans = [""] * n

        for length, idx in arr:
            while q and len(q[0]) < length:
                cur = q.popleft()
                q.append(cur + "0")
                q.append(cur + "1")

            if not q or len(q[0]) != length:
                print("NO")
                return

            ans[idx] = q.popleft()

        print("YES")
        print("\n".join(ans))

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("3\n1 2 3\n").startswith("YES"), "sample 1"

# minimum size
assert run("1\n1\n").startswith("YES"), "single word"

# impossible because only two length-1 strings exist
assert run("3\n1 1 1\n") == "NO\n", "too many short words"

# all length-2 strings fit exactly
res = run("4\n2 2 2 2\n")
assert res.startswith("YES"), "all depth-2 leaves"

# impossible due to Kraft bound
assert run("5\n1 2 2 2 2\n") == "NO\n", "tree exhausted"

# boundary growth
res = run("8\n3 3 3 3 3 3 3 3\n")
assert res.startswith("YES"), "full depth-3 tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`| CÓ | Đầu vào hợp lệ nhỏ nhất | 
|`3 / 1 1 1`| KHÔNG | Chỉ tồn tại hai chuỗi nhị phân có độ dài 1 | 
|`4 / 2 2 2 2`| CÓ | Toàn bộ cấp độ thử có thể được điền chính xác | 
|`5 / 1 2 2 2 2`| KHÔNG | Tiền tố chặn lá có sẵn | 
|`8 / 3 3 3 3 3 3 3 3`| CÓ | Cây nhị phân hoàn chỉnh ở độ sâu 3 | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3
1 1 1
```Thuật toán mở rộng chuỗi trống thành`0`Và`1`. Nó sử dụng cả hai cho hai yêu cầu đầu tiên. Hàng đợi trở nên trống trước yêu cầu thứ ba. Vì không còn cây con nào chưa được sử dụng nên thuật toán sẽ in chính xác:```
NO
```Bây giờ hãy kiểm tra độ dài bằng nhau được lặp lại:```
4
2 2 2 2
```Thuật toán liên tục mở rộng các tiền tố cho đến độ sâu 2:```
00, 01, 10, 11
```Mỗi người được chọn đúng một lần. Vì các nút ở cùng độ sâu không thể là tiền tố của nhau nên việc xây dựng thành công. 

Cuối cùng, hãy xem xét trường hợp các từ ngắn tiêu tốn dung lượng tri:```
5
1 2 2 2 2
```Sau khi chọn một từ có độ dài 1, chỉ còn lại một nửa số tri. Nửa đó chứa chính xác hai lá có độ sâu 2. Thuật toán gán thành công hai từ có độ dài 2, sau đó hàng đợi trở nên trống trong khi các yêu cầu vẫn còn. Nó xuất ra:```
NO
```Điều này phù hợp với giới hạn tổ hợp thực tế của trie nhị phân.
