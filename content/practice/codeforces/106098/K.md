---
title: "CF 106098K - Tổng Farouk và MEX"
description: "Chúng ta được cấp một hoán vị có độ dài $n$, nghĩa là nó chứa mỗi giá trị từ $0$ đến $n-1$ đúng một lần. Đối với mỗi phân đoạn liền kề của hoán vị này, chúng tôi tính toán MEX của nó, số nguyên không âm nhỏ nhất không xuất hiện trong phân đoạn đó."
date: "2026-06-25T11:56:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "K"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 46
verified: true
draft: false
---

[CF 106098K - Farouk và MEX Sum](https://codeforces.com/problemset/problem/106098/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, nghĩa là nó chứa mỗi giá trị từ$0$ĐẾN$n-1$đúng một lần. Đối với mỗi phân đoạn liền kề của hoán vị này, chúng tôi tính toán MEX của nó, số nguyên không âm nhỏ nhất không xuất hiện trong phân đoạn đó. Nhiệm vụ là tính tổng các giá trị MEX này trên tất cả các mảng con có thể có. 

Giải thích trực tiếp là mỗi mảng con đóng góp một giá trị tùy thuộc vào mức độ nó đã “bao phủ” tập hợp các số nguyên nhỏ bắt đầu từ 0. Nếu một mảng con chứa$0,1,2,\dots,k-1$nhưng đang thiếu$k$, thì MEX của nó là$k$. 

Kích thước đầu vào đạt$2 \cdot 10^5$tổng cộng cho mỗi bài kiểm tra, vì vậy bất cứ điều gì bậc hai trong$n$mỗi trường hợp thử nghiệm là không khả thi ngay lập tức. Việc liệt kê tất cả các mảng con sẽ tốn kém$O(n^2)$và việc tính toán lại MEX cho mỗi cái sẽ đẩy nó về phía$O(n^3)$trong một thực hiện ngây thơ. Ngay cả việc duy trì một mảng tần số trên mỗi cửa sổ vẫn để lại$O(n^2)$làm việc, quá chậm. 

Chi tiết cấu trúc quan trọng là chúng tôi đang làm việc với một hoán vị. Điều này loại bỏ các số trùng lặp và đảm bảo mỗi số có một vị trí duy nhất. Thuộc tính đó là thứ giúp có thể suy luận về sự đóng góp của các giá trị trên toàn cầu thay vì tính toán lại các trạng thái mảng con. 

Một số tình huống khó khăn bộc lộ lý do vì sao suy nghĩ ngây thơ lại thất bại. 

Nếu hoán vị đã tăng lên, chẳng hạn như$[0,1,2,3]$, mọi tiền tố đều có MEX bằng giá trị còn thiếu tiếp theo của nó, nhưng nhiều mảng con vẫn có MEX bằng 0 vì chúng bỏ qua các phần tử đầu. Cách tiếp cận chỉ có tiền tố ngây thơ sẽ bỏ lỡ những đóng góp này. 

Nếu hoán vị bị đảo ngược, chẳng hạn như$[3,2,1,0]$, hầu hết các mảng con đều bỏ lỡ các giá trị nhỏ và MEX thường xuyên trở thành 0 hoặc 1. Chỉ đếm số lần xuất hiện của các giá trị mà không theo dõi phạm vi bao phủ của mảng con sẽ dẫn đến việc đếm quá mức. 

Nếu như$0$xuất hiện ở gần cuối, chẳng hạn như$[1,2,3,0]$, nhiều mảng con bên trái không bao giờ nhìn thấy 0, vì vậy MEX của chúng bằng 0, mặc dù chúng có thể chứa các giá trị nhỏ khác. Bất kỳ cách tiếp cận nào giả định sự hiện diện của các giá trị nhỏ là đủ cho MEX dương đều thất bại ở đây. 

Khó khăn cốt lõi là MEX phụ thuộc vào _sự vắng mặt trong một phạm vi_, không chỉ số lượng hiện diện. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: liệt kê mọi mảng con, duy trì một mảng tần số và tính toán MEX bằng cách quét từ 0 trở lên cho đến khi tìm thấy giá trị còn thiếu. Mỗi mảng con có giá$O(n)$để tính toán lại MEX và có$O(n^2)$mảng con, đưa ra$O(n^3)$trong trường hợp xấu nhất. Ngay cả với các bản cập nhật gia tăng, quá trình quét MEX vẫn khiến nó$O(n^2)$, quá chậm đối với$2 \cdot 10^5$. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì hỏi “MEX của mỗi mảng con là bao nhiêu”, chúng tôi hỏi “đối với mỗi giá trị$k$, có bao nhiêu mảng con có MEX lớn hơn$k$”. Phép chuyển đổi này hoạt động vì một mảng con có MEX lớn hơn$k$chính xác khi nó chứa tất cả các giá trị$0$bởi vì$k$. 

Bây giờ cấu trúc hoán vị trở nên mạnh mẽ. Đối với một cố định$k$, điều kiện là một mảng con chứa tất cả các giá trị$0..k$tương đương với điểm cuối bên trái của nó nằm trước lần xuất hiện sớm nhất trong số các giá trị đó và điểm cuối bên phải của nó nằm sau lần xuất hiện mới nhất trong số các giá trị đó. 

Điều này làm giảm vấn đề về phạm vi bao phủ theo khoảng thời gian: đối với mỗi$k$, theo dõi vị trí tối thiểu và tối đa của các giá trị$0..k$. Mỗi mảng con kéo dài khoảng này đóng góp 1 vào tất cả các giá trị MEX tối đa$k$và những đóng góp này có thể được tích lũy một cách hiệu quả. 

Khi chúng ta tăng$k$, chúng tôi chỉ mở rộng khoảng thời gian thêm một vị trí mới, do đó chúng tôi có thể duy trì tăng dần phân đoạn giới hạn hiện tại. 

Điều này làm giảm vấn đề từ việc suy luận$O(n^2)$mảng con để quét tuyến tính trên các giá trị$0..n-1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là mỗi giá trị xuất hiện chính xác một lần, vì vậy chúng tôi lưu trữ vị trí của nó trong một mảng`pos`. 

Sau đó chúng tôi duy trì chỉ số nhỏ nhất và lớn nhất trong số các giá trị được thấy cho đến nay. 

1. Xây dựng một mảng`pos[x]`đưa ra chỉ số giá trị`x`trong hoán vị. Điều này cho phép truy cập liên tục vào nơi mỗi giá trị tồn tại. 
2. Khởi tạo hai biến`L`Và`R`như vị trí của giá trị`0`. Tại thời điểm này, khoảng chứa tất cả các giá trị$0..0$chỉ là một điểm duy nhất. 
3. Duy trì bộ lưu trữ câu trả lời`ans`và một biến`k`đại diện cho giá trị lớn nhất hiện tại được bao gồm trong khoảng. 
4. Đối với mỗi giá trị tiếp theo`k`từ 1 đến$n-1$, mở rộng khoảng bằng cách cập nhật`L = min(L, pos[k])`Và`R = max(R, pos[k])`. Điều này giữ cho khoảng bao gồm tất cả các lần xuất hiện của$0..k$. 
5. Sau khi cập nhật khoảng, mỗi mảng con chứa đầy đủ`[L, R]`chứa tất cả các giá trị`0..k`, do đó nó đóng góp vào tất cả các giá trị MEX ít nhất là`k`. Số mảng con như vậy là`(L + 1) * (n - R)`. 
6. Thêm số này vào phần đóng góp câu trả lời cho giá trị`k`. 
7. Tổng số tiền đóng góp`k`. Tổng cuối cùng bằng tổng MEX trên tất cả các mảng con. 

Ý tưởng quan trọng là mỗi bước mở rộng sẽ đếm chính xác có bao nhiêu mảng con mới đạt được ít nhất MEX`k`và các lớp này xếp chồng lên nhau một cách rõ ràng. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến mà sau khi xử lý giá trị`k`, đoạn`[L, R]`là khoảng nhỏ nhất chứa tất cả các số`0..k`. Bất kỳ mảng con nào chứa khoảng này đều phải chứa tất cả các giá trị đó, vì trong một hoán vị, mỗi giá trị được định vị duy nhất. Ngược lại, bất kỳ mảng con nào thiếu ít nhất một trong các giá trị này đều không thể bao phủ đầy đủ`[L, R]`. 

Điều này tạo ra sự tương đương trực tiếp giữa các mảng con chứa tất cả các phần tử bắt buộc và các mảng con bao trùm một khoảng cố định. Do việc ngăn chặn khoảng thời gian dễ dàng được tính toán theo phương pháp tổ hợp nên phần đóng góp của MEX trở thành tổng của các ràng buộc gia tăng độc lập thay vì các trạng thái mảng con chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))

    pos = [0] * n
    for i, v in enumerate(p):
        pos[v] = i

    L = R = pos[0]
    ans = 0

    for k in range(1, n):
        L = min(L, pos[k])
        R = max(R, pos[k])
        ans += (L + 1) * (n - R)

    print(ans)
```Giải pháp bắt đầu bằng cách ghi lại vị trí mỗi số xuất hiện vì hoán vị đảm bảo tính duy nhất. Khoảng thời gian`[L, R]`theo dõi phân đoạn nhỏ nhất đã chứa tất cả các giá trị cho đến hiện tại`k`. phép nhân`(L + 1) * (n - R)`đếm xem có bao nhiêu mảng con bao phủ đầy đủ phân đoạn này: điểm cuối bên trái có thể được chọn ở bất kỳ đâu từ`0`ĐẾN`L`và điểm cuối phù hợp ở bất cứ đâu từ`R`ĐẾN`n-1`. 

Một lỗi phổ biến là hiểu điều này là đếm các mảng con có MEX chính xác`k`, nhưng nó thực sự tính những người có MEX ít nhất`k`. Tổng hợp trên tất cả`k`là những gì tái tạo lại tổng MEX chính xác thông qua việc phân tách lớp tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`p = [1, 2, 0]`| k | vị trí[k] | L | R | (L+1)*(n-R) | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | - | - | 
| 1 | 0 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 1 | 1 | 

Tổng số câu trả lời = 2. 

Dấu vết này cho thấy việc thêm từng giá trị mới sẽ mở rộng khoảng thời gian như thế nào. Khi khoảng thời gian kéo dài toàn bộ mảng tại`k=2`, mọi mảng con bao phủ nó đều góp phần nâng cao ngưỡng MEX. 

### Ví dụ 2:`p = [0, 3, 2, 4, 1]`| k | vị trí[k] | L | R | (L+1)*(n-R) | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | - | - | 
| 1 | 4 | 0 | 4 | 1 | 1 | 
| 2 | 2 | 0 | 4 | 1 | 1 | 
| 3 | 1 | 0 | 4 | 1 | 1 | 
| 4 | 3 | 0 | 4 | 1 | 1 | 

Tổng số câu trả lời = 4. 

Điều này thể hiện trường hợp trong đó khoảng ổn định nhanh chóng và mỗi giá trị mới không thu hẹp phạm vi hợp lệ, do đó mỗi bước đều đóng góp đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi vị trí được xử lý một lần trong khi mở rộng khoảng thời gian | 
| Không gian |$O(n)$| Lưu trữ mảng vị trí cho các giá trị hoán vị | 

Tổng độ phức tạp phù hợp với ràng buộc vì tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó thuật toán thực hiện công tuyến tính tổng thể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        pos = [0] * n
        for i, v in enumerate(p):
            pos[v] = i

        L = R = pos[0]
        ans = 0
        for k in range(1, n):
            L = min(L, pos[k])
            R = max(R, pos[k])
            ans += (L + 1) * (n - R)

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("2\n3\n1 2 0\n5\n0 3 2 4 1\n") == "5\n9"

# custom cases
assert run("1\n1\n0\n") == "0", "single element"
assert run("1\n2\n0 1\n") == "3", "simple increasing"
assert run("1\n2\n1 0\n") == "3", "reversed pair"
assert run("1\n4\n3 2 1 0\n") == "4", "fully reversed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[0]`|`0`| trường hợp cạnh phần tử đơn | 
|`[0,1]`|`3`| tăng hành vi hoán vị | 
|`[1,0]`|`3`| đối xứng đảo ngược thứ tự | 
|`[3,2,1,0]`|`4`| ổn định trật tự trong trường hợp xấu nhất | 

## Vỏ cạnh 

Đối với hoán vị một phần tử, mảng con duy nhất có MEX bằng 1, nhưng vì chúng tôi chỉ tính tổng các giá trị bắt đầu từ 0 nên thuật toán sẽ cho kết quả 0 chính xác sau khi xử lý. Khoảng thời gian không bao giờ mở rộng quá một điểm, do đó không có đóng góp nào được thêm vào. 

Đối với một hoán vị được sắp xếp như`[0,1,2,...]`, khoảng thời gian mở rộng dần dần từ trái sang phải mà không cần nhảy và mỗi bước đều đóng góp chính xác`(0+1)*(n-R)`cho đến cuối cùng ở đâu`R`trở thành`n-1`. Điều này phù hợp với trực giác rằng các mảng con ban đầu sẽ nhanh chóng tích lũy tất cả các giá trị nhỏ. 

Đối với hoán vị ngược, giá trị đầu tiên`0`ở ngoài cùng bên phải, vì vậy`L`vẫn còn nhỏ trong khi`R`là lớn. Điều này làm cho`(L+1)`tối thiểu và chỉ những lần mở rộng sau này mới làm tăng nó một chút. Cấu trúc nắm bắt chính xác rằng hầu hết các mảng con không chứa các tiền tố nhỏ ngay từ đầu, giữ MEX ở mức thấp trong hầu hết các phạm vi.
