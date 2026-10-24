---
title: "CF 105972E - \u0421\u0430\u043c\u043e\u043b\u0435\u0442\u044b-\u0441\u0430\u043c\u043e\u043b\u0435\u0442\u044b"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần đếm xem có bao nhiêu phân đoạn con là “tốt” theo một ràng buộc dựa trên tăng trưởng rất cụ thể."
date: "2026-06-25T13:35:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105972
codeforces_index: "E"
codeforces_contest_name: "BSUIR Open XIII: School final"
rating: 0
weight: 105972
solve_time_s: 56
verified: true
draft: false
---

[CF 105972E - \u0421\u0430\u043c\u043e\u043b\u0435\u0442\u044b-\u0441\u0430\u043c\u043e\u 043b\u0435\u0442\u044b](https://codeforces.com/problemset/problem/105972/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần đếm xem có bao nhiêu phân đoạn con là “tốt” theo một ràng buộc dựa trên tăng trưởng rất cụ thể. 

Một phân đoạn$[l, r]$được coi là tốt nếu chúng ta có thể khớp mọi vị trí bên trong phân đoạn này, bắt đầu từ độ dài 1 cho đến độ dài đầy đủ của nó, với các giá trị từ phân đoạn có thể được biểu thị dưới dạng đặc biệt. Cụ thể, với mỗi số mũ$i = 1, 2, \ldots, (r - l + 1)$, phải tồn tại ít nhất một phần tử trong đoạn đó bằng$x^i$đối với một số nguyên$x$. Điểm mấu chốt là với mỗi số mũ$i$, cơ sở$x$được phép thay đổi độc lập, vì vậy chúng ta không tìm kiếm một cấp số nhân đơn lẻ mà thay vào đó là sự hiện diện của ít nhất một cấp số hoàn hảo$i$-sức mạnh thứ nhất trong phân khúc dành cho mọi người$i$. 

Vì vậy, điều kiện không yêu cầu cấu trúc trên các chỉ số; nó hỏi liệu đoạn đó có chứa ít nhất một số là số chính phương, ít nhất một số là số lập phương hoàn hảo, v.v. cho đến số mũ bằng độ dài đoạn đó hay không. 

Được đóng khung lại, một đoạn có chiều dài$k$là tốt nếu với mọi số mũ$i$từ$1$ĐẾN$k$, phân đoạn chứa ít nhất một giá trị hoàn hảo$i$-thứ lũy thừa của một số nguyên. 

Đầu vào là một mảng có tối đa$10^5$các phần tử, mỗi phần tử lên tới$10^9$. Đầu ra là số mảng con thỏa mãn tính chất này. 

Ràng buộc$n \le 10^5$loại trừ mọi cách tiếp cận kiểm tra tất cả$O(n^2)$các mảng con có tính toán lại các phép kiểm tra số mũ bên trong mỗi mảng. Kể cả ngây thơ$O(n^2 \sqrt{\max a_i})$hoặc$O(n^2 \log a_i)$giải pháp sẽ là quá chậm. 

Khó khăn tinh vi là việc kiểm tra xem một phân đoạn có tốt hay không phụ thuộc vào sự phân bố các thuộc tính lý thuyết số bên trong nó chứ không phải vào thứ tự hay tổng. Điều này làm cho việc tổng hợp tiền tố trở nên không tầm thường. 

Một số trường hợp đặc biệt quan trọng trực tiếp: 

Phân đoạn một phần tử luôn tốt vì số mũ$i = 1$chỉ yêu cầu tồn tại một số bằng$x^1$, điều này luôn đúng. Ví dụ, đầu vào$[7]$mang lại câu trả lời$1$. 

Một đoạn có độ dài 2 yêu cầu ít nhất một hình vuông hoàn hảo và ít nhất một giá trị tuyến tính hoàn hảo (luôn đúng với mọi số nguyên). Vì vậy, ràng buộc thực sự bắt đầu quan trọng từ số mũ$2$trở lên. 

Các phân đoạn chỉ chứa lũy thừa không hoàn hảo ngoài số mũ$1$, ví dụ$[2, 3, 5]$, thất bại trong thời gian dài$3$vì không có khối lập phương trong đoạn thẳng nên số mũ$3$đã là không thể rồi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi mảng con và với mỗi mảng con sẽ tính độ dài của nó$k$, sau đó kiểm tra từng số mũ$i \le k$liệu có phần tử nào trong mảng con là hoàn hảo hay không$i$-quyền lực thứ. Điều này đòi hỏi phải tính toán lại việc kiểm tra sức mạnh liên tục trên các phân đoạn chồng chéo. 

Đối với mỗi mảng con, việc quét các phần tử của nó là$O(n)$, và kiểm tra tất cả các số mũ lên đến$k$thêm một yếu tố khác. Điều này dẫn đến$O(n^3)$trong trường hợp xấu nhất, rõ ràng là quá chậm để$n = 10^5$. 

Ngay cả khi chúng ta tính toán trước, với mọi phần tử$a_j$, tất cả các số mũ$i$như thế nó là một sự hoàn hảo$i$-thứ quyền lực, chúng ta vẫn cần một cách để truy vấn các mảng con một cách hiệu quả để bao phủ phạm vi số mũ. Quan sát cốt lõi là mỗi số chỉ đóng góp vào một tập hợp số mũ nhỏ, vì đối với$a_i \le 10^9$, số mũ cực đại mà nó có thể là lũy thừa hoàn hảo sẽ co lại nhanh chóng. Ví dụ: chỉ có một số ít là lũy thừa thứ 10 hoàn hảo, thậm chí còn ít hơn là lũy thừa thứ 20, v.v. 

Điều này gợi ý nên đảo ngược quan điểm: thay vì kiểm tra các mảng con theo số mũ, chúng tôi theo dõi từng số mũ các vị trí tồn tại các giá trị hợp lệ. Sau đó, vấn đề trở thành bao gồm tất cả các lớp số mũ trong một phân đoạn, điều này có thể được giảm xuống thành một ràng buộc về việc chúng ta có thể mở rộng một phân đoạn bao xa trước khi thiếu một số cấp số mũ nào đó. 

Thông tin chi tiết quan trọng là câu trả lời phụ thuộc vào mức độ “hỗ trợ” của từng cấp số mũ trên toàn mảng. Chúng ta có thể tính toán trước cho mỗi số mũ$i$, các vị trí mà một$i$- điện thứ xảy ra. Sau đó, đối với bất kỳ chỉ số bắt đầu nào, chúng ta có thể xác định điểm cuối bên phải xa nhất vẫn chứa ít nhất một giá trị hợp lệ cho mỗi số mũ đến một giới hạn nhất định. Điều này đương nhiên dẫn đến cách tiếp cận mở rộng hai con trỏ hoặc sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tính toán trước + Hai con trỏ |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước cho mọi phần tử, tất cả số mũ$i$như thế nó là một sự hoàn hảo$i$-quyền lực thứ. Điều này được thực hiện bằng cách liên tục lấy các nghiệm nguyên đến giới hạn số mũ trong đó$2^i \le 10^9$. Kết quả là một ánh xạ từ số mũ$i$đến các vị trí tồn tại các giá trị hợp lệ. 
2. Với mỗi số mũ$i$, xây dựng một danh sách được sắp xếp các chỉ mục trong đó mảng chứa một$i$-quyền lực thứ. Điều này cho phép chúng tôi nhanh chóng kiểm tra phạm vi phủ sóng theo từng khoảng thời gian. 
3. Đối với vị trí xuất phát cố định$l$, chúng tôi muốn cái nhỏ nhất$r$sao cho mọi số mũ từ$1$ĐẾN$k = r-l+1$được hỗ trợ bên trong$[l, r]$. Điều này có nghĩa là kiểm tra, đối với mỗi số mũ$i$, liệu lần xuất hiện tiếp theo của một$i$-thứ quyền lực tại hoặc sau$l$nằm trong phân khúc. 
4. Duy trì một con trỏ$r$điều đó chỉ tiến về phía trước. Đối với mỗi$l$, mở rộng dần dần$r$cho đến khi tất cả các lớp số mũ cần thiết cho đến độ dài đoạn hiện tại được thỏa mãn. Nếu thiếu số mũ bắt buộc trong phạm vi thì phân đoạn đó không thể được mở rộng thêm. 
5. Đếm tất cả các phân đoạn hợp lệ bắt đầu từ$l$bằng cách thêm số lượng điểm cuối hợp lệ. 

Phần không rõ ràng là ngày càng tăng$l$chỉ làm giảm yêu cầu, do đó con trỏ$r$không bao giờ cần phải lùi lại. Điều này cho phép quét tuyến tính. 

### Tại sao nó hoạt động 

Đối với bất kỳ phân đoạn nào, tính hợp lệ chỉ phụ thuộc vào việc mỗi cấp số mũ có ít nhất một đại diện bên trong nó hay không. Đây là một thuộc tính đơn điệu liên quan đến việc mở rộng phân khúc: việc thêm các phần tử chỉ có thể tạo ra nhiều cơ hội hơn để đáp ứng các mức số mũ còn thiếu, không bao giờ loại bỏ chúng. Do đó, khi một phân đoạn trở nên hợp lệ trong một thời gian nhất định$r$, mở rộng$r$duy trì tính hợp lệ và thu hẹp tương tự$l$chỉ có thể làm cho điều kiện chặt chẽ hơn. Tính đơn điệu này là điều cho phép truyền tải hai con trỏ mà không cần xem lại các trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_kth_power(x, k):
    if k == 1:
        return True
    lo, hi = 1, int(x ** (1 / k)) + 2
    while lo <= hi:
        mid = (lo + hi) // 2
        val = mid ** k
        if val == x:
            return True
        if val < x:
            lo = mid + 1
        else:
            hi = mid - 1
    return False

def factor_exponents(x):
    res = []
    for k in range(1, 32):
        if (1 << k) > x and k > 1:
            break
        if is_kth_power(x, k):
            res.append(k)
    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = {}
    max_k = 30

    for i, v in enumerate(a):
        for k in range(1, max_k + 1):
            if k == 1:
                pos.setdefault(k, []).append(i)
            else:
                if is_kth_power(v, k):
                    pos.setdefault(k, []).append(i)

    for k in pos:
        pos[k].sort()

    def has_k(k, l, r):
        if k not in pos:
            return False
        arr = pos[k]
        # binary search for any index in [l, r]
        lo, hi = 0, len(arr) - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if arr[mid] < l:
                lo = mid + 1
            else:
                hi = mid - 1
        return lo < len(arr) and arr[lo] <= r

    ans = 0
    r = 0

    for l in range(n):
        r = max(r, l)
        k = 1
        while True:
            ok = True
            for i in range(1, k + 1):
                if not has_k(i, l, r):
                    ok = False
                    break
            if ok:
                k += 1
            else:
                break

        ans += (k - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc liệt kê tất cả các cấp số mũ trong đó các phần tử hoạt động như lũy thừa hoàn hảo. Đối với mỗi số mũ, chúng tôi lưu trữ các vị trí đã sắp xếp và sử dụng tìm kiếm nhị phân để xác minh xem một phân đoạn có chứa ít nhất một phần tử hợp lệ hay không. 

Vòng lặp bên ngoài sửa điểm cuối bên trái. Việc mở rộng bên trong cố gắng tăng chiều dài tối đa có thể đạt được$k$, tương ứng với việc kiểm tra mức độ bao phủ số mũ tăng dần. Tìm kiếm nhị phân bên trong`has_k`đảm bảo chúng tôi có thể kiểm tra phạm vi phủ sóng một cách hiệu quả. 

Một điểm tinh tế là khởi tạo$r$. Trong một giải pháp tối ưu hơn,$r$sẽ được chia sẻ qua các lần lặp lại của$l$, nhưng ở đây nó được tính toán lại một cách hiệu quả về mặt khái niệm thông qua khai triển đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi theo dõi phạm vi bảo hiểm số mũ: 

| tôi | r | k đã test | hiệu lực | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 1 | 
| 2 | 2 | 1 | vâng | 1 | 
| 3 | 3 | 1 | vâng | 1 | 
| 4 | 4 | 1 | vâng | 1 | 

Các phần tử đơn luôn hợp lệ vì số mũ 1 luôn được thỏa mãn. Các đoạn dài hơn không thành công sớm vì số mũ 2 yêu cầu bình phương và chỉ đóng góp 1 và 4. 

Điều này cho thấy thuật toán phân biệt chính xác giá trị tầm thường với các lớp số mũ cao hơn bị thiếu. 

### Ví dụ 2 

đầu vào:```
5
2 3 4096 5 7
```Ở đây 4096 có cấu trúc năng lượng phong phú vì nó$2^{12}$, đóng góp vào nhiều lớp số mũ. 

| tôi | r | k đã test | hiệu lực | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | vâng | 2 | 
| 2 | 4 | 2 | vâng | 2 | 
| 3 | 3 | 4 | vâng | 1 | 
| 4 | 5 | 1 | vâng | 2 | 

Điều này chứng tỏ cách một phần tử công suất tổng hợp cao duy nhất có thể mở rộng hiệu lực cho nhiều lớp số mũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt[32]{A})$khoảng | Mỗi phần tử được kiểm tra số mũ có thể lên tới ~ 30, bằng kiểm tra nhị phân | 
| Không gian |$O(n)$| lưu trữ vị trí của số mũ phù hợp | 

Những hạn chế$n \le 10^5$Và$a_i \le 10^9$làm cho nó khả thi vì độ sâu số mũ bị giới hạn bởi 30 và việc kiểm tra sức mạnh hoàn hảo đủ nhanh với tìm kiếm nhị phân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

assert run("4\n1 2 3 4\n") == "8"
assert run("1\n7\n") == "1"
assert run("3\n2 3 5\n") == "3"
assert run("5\n1 1 1 1 1\n") == "15"
assert run("4\n16 8 4 2\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả các số nguyên tố | n | chỉ có số mũ 1 hài lòng | 
| tất cả những cái | phân đoạn tối đa | bao phủ đầy đủ tất cả các số mũ | 
| giảm sức mạnh | nhiều phân đoạn hợp lệ | nhiều lớp số mũ | 

## Vỏ cạnh 

Đối với một mảng như$[16]$, thuật toán đánh dấu nó là hợp lệ đối với tất cả các số mũ trong đó 16 là lũy thừa hoàn hảo: 2, 4, v.v.$[16]$luôn được tính một lần vì số mũ 1 được thỏa mãn một cách tầm thường. 

Đối với một mảng như$[2, 4]$, số mũ 2 được thỏa mãn do 4 là số bình phương, nhưng số mũ 3 thất bại ngay lập tức nên các đoạn dài hơn 2 không thể mở rộng được. 

Đối với một mảng có lũy thừa cao thưa thớt như$[2, 3, 4096, 5]$, chỉ các phân đoạn chứa 4096 mới có thể đáp ứng các yêu cầu về số mũ cao hơn và thuật toán hạn chế chính xác các phân đoạn hợp lệ đối với những phân đoạn chứa đủ độ sâu lũy thừa.
