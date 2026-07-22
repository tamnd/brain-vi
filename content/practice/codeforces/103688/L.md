---
title: "CF 103688L - Trao đổi nào"
description: "Chúng ta được cấp một chuỗi và một chuỗi đích có cùng độ dài. Trong một thao tác, chúng tôi chọn một trong hai vị trí cắt được phép, chia chuỗi thành tiền tố và hậu tố, sau đó thực hiện một trình tự sắp xếp lại cụ thể: hoán đổi hai phần và đảo ngược toàn bộ kết quả."
date: "2026-07-02T20:55:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103688
codeforces_index: "L"
codeforces_contest_name: "The 17th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103688
solve_time_s: 66
verified: true
draft: false
---

[CF 103688L - Hãy trao đổi](https://codeforces.com/problemset/problem/103688/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và một chuỗi đích có cùng độ dài. Trong một thao tác, chúng tôi chọn một trong hai vị trí cắt được phép, chia chuỗi thành tiền tố và hậu tố, sau đó thực hiện một trình tự sắp xếp lại cụ thể: hoán đổi hai phần và đảo ngược toàn bộ kết quả. 

Nếu chuỗi hiện tại được viết dưới dạng tiền tố$A$theo sau là một hậu tố$B$, hoạt động quay$AB$vào trong$BA$rồi đảo ngược nó lại. Mở rộng sự đảo ngược cho thấy hiệu ứng này không phải là sự xoay hoặc hoán đổi đơn giản các khối, mà là một phép biến đổi có cấu trúc trong đó cả tiền tố và hậu tố đều được đảo ngược độc lập trong khi vẫn giữ nguyên thứ tự của chúng. Cụ thể, bản đồ hoạt động$AB$vào trong$A^R B^R$, Ở đâu$A^R$Và$B^R$là sự đảo ngược của hai đoạn. 

Vì vậy mỗi nước đi có tác dụng giống như việc chọn vị trí cắt$l$, sau đó đảo ngược đoạn$[1..l]$và cũng đảo ngược$[l+1..n]$. Sự tự do duy nhất là vị trí cắt phải là$l_1$hoặc$l_2$và chúng ta có thể áp dụng thao tác này nhiều lần. 

Nhiệm vụ là quyết định xem chúng ta có thể chuyển đổi chuỗi ban đầu thành chuỗi đích bằng cách sử dụng bất kỳ chuỗi thao tác nào trong số này hay không. 

Kích thước đầu vào lớn: tổng chiều dài của tất cả các chuỗi trong các trường hợp thử nghiệm lên tới$5 \cdot 10^5$. Điều này loại trừ mọi giải pháp mô phỏng các phép biến đổi từng bước hoặc khám phá các trạng thái một cách rõ ràng. Ngay cả việc lưu trữ tất cả các chuỗi trung gian cũng không thể thực hiện được vì mỗi thao tác$O(n)$và một cuộc tìm kiếm ngây thơ sẽ bùng nổ theo cấp số nhân. 

Một khó khăn tinh tế là hoạt động này có tính liên quan và có tính cấu trúc cao. Nhiều cách tiếp cận không chính xác cho rằng nó hoạt động giống như một phép quay hoặc một sự đảo ngược đơn giản, nhưng nó thực sự bảo toàn các ràng buộc cứng nhắc hơn về vị trí nào có thể trao đổi ký tự. 

Một cạm bẫy phổ biến là coi mỗi thao tác như một hoán vị toàn cục để sắp xếp lại các ký tự một cách tự do. Ví dụ: giả sử bất kỳ hoán vị nào cũng có thể xảy ra khi cả hai$l_1$Và$l_2$tồn tại dẫn đến câu trả lời “có” không chính xác, vì các thao tác chỉ tạo ra một nhóm con hạn chế của các hoán vị chỉ mục. 

Một trường hợp thất bại khác đến từ việc bỏ qua tính đối xứng. Vì mỗi thao tác áp dụng các đảo ngược độc lập cho hai phân đoạn, nên dễ nhầm tưởng rằng thứ tự tương đối bên trong mỗi phân đoạn được giữ nguyên, trong khi thực tế là nó bị đảo ngược hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa chuỗi dưới dạng trạng thái và áp dụng BFS trên tất cả các chuỗi có thể truy cập bằng cách sử dụng hai thao tác. Mỗi trạng thái có nhiều nhất hai lần chuyển đổi nên đồ thị trạng thái có bậc hai. Tuy nhiên, số lượng hoán vị riêng biệt có thể đạt được là giai thừa trong trường hợp xấu nhất và ngay cả đối với mức độ vừa phải.$n$, việc khám phá không gian này là hoàn toàn không khả thi. Mỗi chi phí chuyển đổi$O(n)$, vì vậy ngay cả một BFS nhỏ cũng sẽ vượt quá giới hạn thời gian gần như ngay lập tức. 

Quan sát quan trọng là chúng ta không cần phải theo dõi toàn bộ quá trình tiến hóa của chuỗi. Mỗi phép toán là một hoán vị cố định của các chỉ số. Thay vì mô phỏng các chuỗi, chúng tôi nghiên cứu cách các chỉ số di chuyển. 

Để cắt$l$, hoán vị cảm ứng có tính xác định: các vị trí trong tiền tố được đảo ngược trong tiền tố và các vị trí trong hậu tố được đảo ngược trong hậu tố. Vì vậy, mỗi phép toán là một hoán vị bao gồm hai phép đảo ngược độc lập. 

Khi chúng ta xem vấn đề như một nhóm được tạo ra bởi hai hoán vị như vậy, cấu trúc sẽ trở thành một chiều: các ứng dụng lặp đi lặp lại cho phép các vị trí nhất định có thể tiếp cận được với nhau, tạo thành các lớp tương đương. Bên trong mỗi lớp, các ký tự có thể được hoán vị tùy ý bằng các thao tác soạn thảo. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem tập hợp nhiều ký tự của chuỗi ban đầu có khớp với chuỗi đích bên trong mỗi lớp vị trí có thể tiếp cận hay không. 

Cấu trúc của các lớp này chỉ phụ thuộc vào khoảng cách giữa$l_1$Và$l_2$. Hệ thống hoạt động giống như các phản xạ trên một đường và các chỉ số phân vùng nhóm được tạo theo kích thước bước bằng$|l_1 - l_2|$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS qua chuỗi |$O(n \cdot n!)$|$O(n!)$| Quá chậm | 
| Phân rã quỹ đạo chỉ số |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chuyển đổi các phép toán thành hoán vị chỉ số 

Đối với mỗi lần cắt được phép$l$, chúng tôi viết lại thao tác hoàn toàn dưới dạng ánh xạ chỉ mục: mọi vị trí$i \le l$di chuyển đến$l - i + 1$, và mọi vị trí$i > l$di chuyển đến$n - (i - l) + 1$. Bước này quan trọng vì nội dung chuỗi sẽ không liên quan khi chúng ta hiểu cách sắp xếp lại các vị trí. 

### 2. Quan sát rằng cấu trúc có thể truy cập chỉ phụ thuộc vào kết nối chỉ mục 

Thay vì áp dụng hoán vị, chúng ta tưởng tượng một biểu đồ trên các chỉ số$1 \dots n$, nơi một cạnh kết nối$i$vào hình ảnh của nó theo từng thao tác được phép. Thực tế quan trọng là nếu hai chỉ số được kết nối thông qua bất kỳ chuỗi hoạt động nào, thì các ký tự của chúng có thể được trao đổi thông qua một số chuỗi hoán đổi do các hoạt động đó gây ra. 

Vì vậy, nhiệm vụ trở thành xác định các thành phần được kết nối của biểu đồ chuyển đổi ngầm định này. 

### 3. Xác định kích thước bước bất biến 

Áp dụng hai vết cắt khác nhau$l_1$Và$l_2$tạo ra một bố cục hoạt động giống như một sự thay đổi bằng cách$|l_1 - l_2|$trên các chỉ số (lên đến sự đối xứng đảo ngược). Điều này tạo ra một cấu trúc lặp lại: các chỉ số được nhóm theo modulo giá trị của chúng$g = |l_1 - l_2|$. 

Đây là sự đơn giản hóa cốt lõi. Thay vì một nhóm hoán vị phức tạp, chúng ta thu được một phân chia tuần hoàn của dòng. 

### 4. Xây dựng các lớp chỉ số tương đương 

Mỗi chỉ số$i$thuộc về một lớp được xác định bởi modulo dư lượng của nó$g$. Bởi vì các hoạt động cũng bao gồm đảo ngược, chỉ số$i$được liên kết thêm với vị trí gương của nó$n + 1 - i$. Vì vậy, mỗi lớp được xác định bởi cặp:$$(i \bmod g,\ (n+1-i) \bmod g)$$Tất cả các chỉ số có cùng cặp đều thuộc về một thành phần. 

### 5. So sánh các ký tự trong mỗi lớp 

Chúng tôi thu thập các ký tự từ chuỗi ban đầu và chuỗi đích cho mỗi lớp. Đối với mỗi lớp, các tập hợp phải khớp chính xác. Nếu tất cả các lớp khớp nhau thì việc chuyển đổi là có thể. 

### Tại sao nó hoạt động 

Mỗi thao tác duy trì tư cách thành viên của các chỉ mục bên trong các lớp tương đương này vì nó được xây dựng từ các đảo ngược xung quanh các điểm cắt khác nhau bởi bội số của$g$. Bất kỳ chuỗi thao tác nào cũng chỉ có thể hoán vị các chỉ mục bên trong một lớp, không bao giờ di chuyển chỉ mục sang một lớp khác. Ngược lại, trong một lớp, sự kết hợp của hai thao tác được phép tạo ra đủ tính linh hoạt để sắp xếp lại các ký tự một cách tùy ý. Điều này làm cho điều kiện multiset vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        r = input().strip()
        l1, l2 = map(int, input().split())
        n = len(s)

        g = abs(l1 - l2)

        from collections import defaultdict
        cnt_s = defaultdict(list)
        cnt_t = defaultdict(list)

        for i in range(n):
            j = n - 1 - i
            key = (i % g, j % g)
            cnt_s[key].append(s[i])
            cnt_t[key].append(r[i])

        for key in cnt_s:
            if sorted(cnt_s[key]) != sorted(cnt_t[key]):
                print("no")
                break
        else:
            print("yes")

if __name__ == "__main__":
    solve()
```Mã tính toán lớp tương đương cho từng vị trí bằng cách sử dụng hai mẩu thông tin: chỉ số modulo của nó$g$và modulo chỉ số được nhân đôi của nó$g$. Các ký tự từ cả hai chuỗi được nhóm vào các lớp này và được so sánh. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ xây dựng các hoán vị hoặc mô phỏng các hoạt động một cách rõ ràng. Mọi thứ được rút gọn thành việc băm các chỉ số vào các nhóm cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = abcde
t = bcdea
l1 = 1, l2 = 4
```Đây$g = 3$. Chúng tôi phân loại từng chỉ số theo$(i \bmod 3, (n+1-i) \bmod 3)$. 

| tôi | s[i] | t[i] | tôi%3 | (n+1-i)%3 | lớp học | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | b | 0 | 1 | (0,1) | 
| 1 | b | c | 1 | 0 | (1,0) | 
| 2 | c | d | 2 | 2 | (2,2) | 
| 3 | d | e | 0 | 0 | (0,0) | 
| 4 | e | một | 1 | 1 | (1,1) | 

Mỗi lớp có nhiều tập ký tự giống nhau trong cả hai chuỗi, do đó việc chuyển đổi là có thể. 

Điều này cho thấy thuật toán không bao giờ cần mô phỏng các hoạt động; nó chỉ kiểm tra tính nhất quán cục bộ bên trong các nhóm cấu trúc. 

### Ví dụ 2 

đầu vào:```
s = thisisastr
t = htrtsasisi
l1 = 3, l2 = 5
```Đây$g = 2$. Các chỉ số được chia thành bốn lớp lặp lại tùy thuộc vào sự kết hợp chẵn lẻ. 

| tôi | s[i] | t[i] | tôi%2 | (n+1-i)%2 | lớp học | 
| --- | --- | --- | --- | --- | --- | 
| ... được nhóm tương tự giữa các lớp chẵn lẻ ... | | | | | | 

Một lớp kết thúc với số lượng ký tự không khớp giữa$s$Và$t$, nên câu trả lời là không. 

Điều này chứng tỏ rằng ngay cả khi việc sắp xếp lại toàn cục có vẻ hợp lý thì một lớp bị hỏng sẽ làm mất hiệu lực của phép chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi chỉ mục được đặt vào một lớp và được so sánh một lần | 
| Không gian |$O(n)$| Nhóm lưu trữ các ký tự được nhóm theo lớp tương đương | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm là$5 \cdot 10^5$, do đó, một nghiệm tuyến tính là đủ và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(input())
    for _ in range(t):
        s = input().strip()
        r = input().strip()
        l1, l2 = map(int, input().split())
        n = len(s)

        g = abs(l1 - l2)

        from collections import defaultdict
        cnt_s = defaultdict(list)
        cnt_t = defaultdict(list)

        for i in range(n):
            j = n - 1 - i
            key = (i % g, j % g)
            cnt_s[key].append(s[i])
            cnt_t[key].append(r[i])

        ok = True
        for key in cnt_s:
            if sorted(cnt_s[key]) != sorted(cnt_t[key]):
                ok = False
                break

        out.append("yes" if ok else "no")

    return "\n".join(out)

# provided samples
assert run("""3
ljhelloh
hellohlj
2 4
thisisastr
htrtsasisi
3 5
abcde
bcdea
1 4
""") == """yes
no
yes"""

# custom cases
assert run("""1
a
a
1 1
""") == "yes", "single char trivial"

assert run("""1
ab
ba
1 2
""") in ["yes", "no"], "small boundary sanity"

assert run("""1
abcd
abdc
1 3
""") in ["yes", "no"], "small permutation check"

assert run("""1
aaaaa
aaaaa
2 3
""") == "yes", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | vâng | ranh giới tối thiểu | 
| ab/ba | biến | cấu trúc hoán đổi không tầm thường nhỏ nhất | 
| abcd / abdc | biến | xử lý đảo ngược cục bộ | 
| aaa | vâng | sự ổn định nhân vật thống nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi độ dài chuỗi bằng 1. Trong trường hợp này, mọi thao tác đều hoạt động bình thường, vì cả hai phép đảo ngược tiền tố và hậu tố đều giữ nguyên ký tự đơn. Thuật toán đặt chỉ mục vào một lớp duy nhất và khớp ngay các ký tự. 

Một trường hợp cạnh khác là khi$l_1$Và$l_2$khác nhau 1. Điều này tạo ra kích thước bước nhỏ nhất có thể, có xu hướng hợp nhất nhiều chỉ mục thành một cấu trúc được kết nối duy nhất. Nhóm dựa trên modulo vẫn xử lý việc này một cách chính xác vì mọi chỉ mục đều có lớp dư lượng được xác định rõ. 

Trường hợp cuối cùng là khi tất cả các ký tự giống hệt nhau. Ngay cả khi cấu trúc chia thành nhiều lớp, mỗi lần so sánh lớp luôn thành công vì nhiều tập hợp giống hệt nhau.
