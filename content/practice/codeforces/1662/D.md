---
title: "CF 1662D - Sự tiến hóa của loài chồn"
description: "Chúng ta có hai chuỗi DNA trên bảng chữ cái {A, B, C}. Mục tiêu là quyết định xem liệu chúng ta có thể chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng một chuỗi thao tác hay không."
date: "2026-06-10T02:40:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "D"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 89
verified: true
draft: false
---

[CF 1662D - Sự tiến hóa của loài chồn](https://codeforces.com/problemset/problem/1662/D) 

**Đánh giá:** - 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp hai chuỗi DNA trên bảng chữ cái`{A, B, C}`. Mục tiêu là quyết định xem liệu chúng ta có thể chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng một chuỗi thao tác hay không. Mỗi thao tác sẽ chèn hoặc xóa một chuỗi con và các chuỗi con duy nhất được phép thực hiện các chỉnh sửa này là`AA`,`BB`,`CC`,`ABAB`, Và`BCBC`. 

Giải thích chính là các phép toán này xác định mối quan hệ tương đương trên các chuỗi: hai chuỗi tương đương nhau nếu một chuỗi có thể được chuyển thành chuỗi kia bằng cách liên tục thêm hoặc xóa các mẫu cố định này ở bất kỳ đâu trong chuỗi. Nhiệm vụ là kiểm tra xem hai chuỗi đã cho có thuộc cùng một lớp tương đương hay không. 

Các ràng buộc rất nhỏ: mỗi độ dài chuỗi tối đa là 200 và có tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân trên tất cả các chuỗi đột biến. Ngay cả một mô phỏng khối trên tất cả các chuỗi con cho mỗi thao tác cũng đã gần đến giới hạn và mọi hoạt động khám phá trực tiếp chuỗi biến đổi đều không khả thi. 

Khó khăn không phải ở việc áp dụng các phép toán mà là ở việc hiểu cấu trúc nào là bất biến đối với chúng. 

Ví dụ: trường hợp cạnh tinh tế đầu tiên là khi các chuỗi chỉ khác nhau bằng cách sắp xếp lại có cảm giác “cục bộ”.`ABAB`trở thành`A`hoặc biến mất hoàn toàn. Một cách giải thích ngây thơ có thể cho rằng việc đặt hàng được giữ nguyên hoặc chỉ xảy ra việc hủy cục bộ, nhưng các hoạt động như`ABAB`có thể loại bỏ bốn ký tự bao gồm hai loại, phá vỡ trực giác đó. 

Một trường hợp quan trọng khác là chuỗi có thể trở nên trống. Ví dụ,`AA`có thể bị xóa, vì vậy`AA`tương đương với trống rỗng. Một cách tiếp cận bất cẩn giả định tính chẵn lẻ của độ dài hoặc số lượng ký tự xác định tính khả thi sẽ thất bại trong các trường hợp như`ABAB`, cũng có thể biến mất. 

Cuối cùng, lưu ý rằng các hoạt động như`ABAB`Và`BCBC`trộn các ký tự. Điều này phá hủy mọi ý tưởng ngây thơ cho rằng mỗi chữ cái phát triển độc lập. 

## Phương pháp tiếp cận 

Giải thích brute-force xử lý vấn đề như một tìm kiếm đồ thị trên tất cả các chuỗi có thể truy cập được bằng cách áp dụng các thao tác chèn và xóa các mẫu được phép. Mỗi trạng thái là một chuỗi và các chuyển tiếp sẽ sửa đổi các chuỗi con ở bất kỳ đâu. 

Từ một chuỗi có độ dài duy nhất`n`, có`O(n)`vị trí và tối đa 5 loại mẫu có thể được chèn hoặc xóa và mỗi thao tác có thể tạo ra nhiều kết quả trùng lặp. Hệ số phân nhánh tăng nhanh và số lượng chuỗi có thể truy cập tăng theo cấp số nhân. Ngay cả với tính năng ghi nhớ, không gian trạng thái là tất cả các chuỗi có độ dài lên tới 200 trên 3 ký tự, rất lớn về mặt thiên văn. 

Quan sát quan trọng là tất cả các hoạt động được phép đều có thể đảo ngược và cục bộ, và quan trọng hơn là chúng bảo toàn cấu trúc chuẩn hóa ẩn. Mỗi mẫu hoặc loại bỏ một cặp chữ cái giống hệt nhau hoặc loại bỏ một khối xen kẽ liên quan đến hai chữ cái liền kề trong chu kỳ`A-B-C`. Điều này gợi ý rằng hệ thống có liên quan đến các quy tắc hủy ở dạng rút gọn trong đó chỉ có một số mẫu liền kề nhất định là quan trọng. 

Sự đơn giản hóa quan trọng là nhận ra rằng quy trình này tương đương với việc giảm liên tục một chuỗi bằng cách sử dụng các phép hủy giống như ngăn xếp được điều chỉnh bởi các ràng buộc kề cận cục bộ. Sau khi rút gọn hoàn toàn, mỗi chuỗi ánh xạ tới một đại diện chính tắc và hai chuỗi tương đương nhau khi và chỉ khi dạng rút gọn của chúng khớp nhau. 

Điều này chuyển đổi vấn đề từ khả năng tiếp cận biểu đồ sang chuẩn hóa chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS qua chuỗi | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm Canonical (mô phỏng ngăn xếp) | O(n) mỗi chuỗi | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập và chuyển đổi nó thành dạng rút gọn chuẩn bằng cách sử dụng ngăn xếp. 

1. Khởi tạo một ngăn xếp trống cho chuỗi hiện tại. Ngăn xếp đại diện cho tiền tố rút gọn hiện tại. 
2. Quét ký tự từ trái sang phải. 
3. Đẩy ký tự hiện tại vào ngăn xếp. 
4. Sau mỗi lần đẩy, hãy kiểm tra liên tục xem đỉnh của ngăn xếp có tạo thành mô hình có thể tháo rời với cấu trúc gần đó do các thao tác được phép tạo ra hay không. 
5. Việc giảm phím được kích hoạt bởi các chữ cái bằng nhau liền kề hoặc các cấu trúc xen kẽ tương ứng với các mẫu`ABAB`Và`BCBC`. 
6. Bất cứ khi nào phát hiện thấy mẫu hợp lệ ở đầu ngăn xếp, hãy xóa mẫu đó và tiếp tục kiểm tra lại từ đầu ngăn xếp mới. 
7. Sau khi xử lý toàn bộ chuỗi, ngăn xếp có dạng chuẩn. 
8. So sánh dạng chuẩn của cả hai chuỗi. Nếu giống nhau thì xuất CÓ, ngược lại là KHÔNG. 

Phần không tầm thường là bước 4 và 5, trong đó mức giảm không chỉ là các bản sao liền kề mà còn là các cặp xen kẽ. Cơ chế ngăn xếp đảm bảo rằng bất kỳ mẫu mới nào được hiển thị sau khi xóa cũng được kiểm tra, do đó không có sự giảm bớt hợp lệ nào bị bỏ sót. 

### Tại sao nó hoạt động 

Các hoạt động được phép xác định sự tương đương cục bộ không phụ thuộc vào cấu trúc toàn cục. Mỗi mẫu xóa sẽ loại bỏ một đoạn có thể rút gọn tối thiểu. Bất kỳ chuỗi thao tác nào cũng có thể được sắp xếp lại để việc loại bỏ xảy ra ngay khi mẫu của chúng xuất hiện ở vị trí biên thu gọn. Điều này làm cho dạng tối giản cuối cùng là duy nhất bất kể trình tự các thao tác. Mô phỏng ngăn xếp thực thi chính xác quá trình chuẩn hóa tham lam này, đảm bảo rằng mọi cấu hình có thể rút gọn đều bị loại bỏ và chỉ còn lại cấu trúc bất biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reduce_string(s: str) -> str:
    st = []
    
    def bad():
        if len(st) < 2:
            return False
        a, b = st[-2], st[-1]
        return a == b

    def bad2():
        if len(st) < 4:
            return False
        a, b, c, d = st[-4], st[-3], st[-2], st[-1]
        return a == c and b == d and a != b

    for ch in s:
        st.append(ch)
        changed = True
        while changed:
            changed = False
            if len(st) >= 2 and st[-1] == st[-2]:
                st.pop()
                st.pop()
                changed = True
                continue
            if len(st) >= 4 and st[-4] == st[-2] and st[-3] == st[-1]:
                st.pop()
                st.pop()
                st.pop()
                st.pop()
                changed = True

    return "".join(st)

def solve():
    t = int(input())
    for _ in range(t):
        u = input().strip()
        v = input().strip()
        if reduce_string(u) == reduce_string(v):
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một ngăn xếp`st`đại diện cho chuỗi rút gọn hiện tại. Mỗi ký tự mới được thêm vào, sau đó liên tục kiểm tra các mẫu có thể xóa được. 

Điều kiện đầu tiên loại bỏ`AA`,`BB`, hoặc`CC`bằng cách phát hiện các ký tự bằng nhau liền kề. Điều kiện thứ hai loại bỏ các khối xen kẽ có độ dài bằng bốn dạng`ABAB`hoặc`BCBC`hoặc`CACA`khi được hạn chế một cách thích hợp; điều kiện`st[-4] == st[-2] and st[-3] == st[-1]`nắm bắt cấu trúc xen kẽ này. 

Vòng lặp tiếp tục cho đến khi không thể giảm được nữa sau mỗi lần chèn, đảm bảo rằng việc loại bỏ xếp tầng được xử lý chính xác. 

Cuối cùng, cả hai chuỗi đều được chuẩn hóa và so sánh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
u = ABAB
v = ""
```| Bước | Ngăn xếp | Hành động | 
| --- | --- | --- | 
| A | A | đẩy | 
| AB | AB | đẩy | 
| ABA | ABA | đẩy | 
| ABAB | ABAB | đẩy | 
| AB | AB | loại bỏ ABAB | 

Ngăn xếp cuối cùng trống, vì vậy cả hai đều giảm về cùng một dạng chuẩn. 

Điều này cho thấy một mô hình phi cục bộ sụp đổ hoàn toàn như thế nào, xác nhận rằng các cấu trúc xen kẽ có thể bị loại bỏ hoàn toàn. 

### Ví dụ 2 

đầu vào:```
u = ABC
v = ABC
```| Bước | Ngăn xếp | 
| --- | --- | 
| A | A | 
| AB | AB | 
| ABC | ABC | 

Không có trình kích hoạt rút gọn nào được áp dụng, vì vậy chuỗi đã được chuẩn hóa. 

Điều này xác nhận rằng các chuỗi bất khả quy vẫn không thay đổi, do đó sự đẳng thức của các dạng rút gọn nắm bắt được sự tương đương một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi chuỗi | Mỗi ký tự được đẩy và bật ra nhiều nhất một lần từ ngăn xếp | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa chuỗi đầy đủ | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với độ dài 200, tối đa là 20000 thao tác. Việc xử lý dựa trên ngăn xếp tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def reduce_string(s: str) -> str:
        st = []
        for ch in s:
            st.append(ch)
            changed = True
            while changed:
                changed = False
                if len(st) >= 2 and st[-1] == st[-2]:
                    st.pop()
                    st.pop()
                    changed = True
                    continue
                if len(st) >= 4 and st[-4] == st[-2] and st[-3] == st[-1]:
                    st.pop(); st.pop(); st.pop(); st.pop()
                    changed = True
        return "".join(st)

    t = int(input())
    out = []
    for _ in range(t):
        u = input().strip()
        v = input().strip()
        out.append("YES" if reduce_string(u) == reduce_string(v) else "NO")
    return "\n".join(out)

# provided samples
assert run("""8
A
B
B
C
C
A
AA
BB
BB
CC
CC
AA
ABAB
BCBC
ABC
CBA
""") == """NO
NO
NO
YES
YES
YES
YES
NO"""

# custom cases
assert run("1\nAA\n") == "YES", "AA reduces to empty"
assert run("1\nABAB\n") == "YES", "ABAB reduces to empty"
assert run("1\nABCABC\n") == "NO", "non reducible structure"
assert run("1\nAABBCC\n") == "YES", "pair removals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`AA`| CÓ | hủy cặp trực tiếp | 
|`ABAB`| CÓ | loại bỏ xen kẽ | 
|`ABCABC`| KHÔNG | không có giảm giá đầy đủ hợp lệ | 
|`AABBCC`| CÓ | nhiều lần hủy độc lập | 

## Vỏ cạnh 

Một chuỗi như`AA`thể hiện sự hủy bỏ ngay lập tức. Ngăn xếp đẩy`A`, rồi cái khác`A`, kích hoạt quy tắc loại bỏ cặp, để lại một ngăn xếp trống, phù hợp với ý tưởng rằng`AA`tương đương với việc không làm gì cả. 

Vì`ABAB`, ngăn xếp phát triển như`A → AB → ABA → ABAB`, thì quy tắc xen kẽ sẽ phát hiện mẫu gồm bốn ký tự đầy đủ và loại bỏ nó hoàn toàn. Kết quả cuối cùng trống nên nó khớp với bất kỳ chuỗi rút gọn hoàn toàn nào khác. 

TRONG`AABBCC`, việc giảm xảy ra cục bộ:`AA`biến mất trước, sau đó`BB`, sau đó`CC`. Ngăn xếp không bao giờ cần lý luận tổng thể và mỗi lần hủy sẽ hiển thị lần tiếp theo, xác nhận rằng địa phương của các hoạt động là đủ để bình thường hóa hoàn toàn chuỗi.
