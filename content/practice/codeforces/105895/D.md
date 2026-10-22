---
title: "CF 105895D - Trò chơi vua (Phiên bản cứng)"
description: "Chúng ta được cho một mảng các số nguyên dương. Mỗi truy vấn tập trung vào một phân đoạn liền kề của mảng này và đối với phân đoạn đó, chúng ta được phép chọn bất kỳ chuỗi con nào (không nhất thiết phải liền kề nhưng giữ nguyên thứ tự) để tạo thành một “mảng trò chơi”."
date: "2026-06-21T15:12:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105895
codeforces_index: "D"
codeforces_contest_name: "The 21st Southeast University Programming Contest (Summer)"
rating: 0
weight: 105895
solve_time_s: 70
verified: true
draft: false
---

[CF 105895D - Trò chơi vua (Phiên bản cứng)](https://codeforces.com/problemset/problem/105895/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Mỗi truy vấn tập trung vào một phân đoạn liền kề của mảng này và đối với phân đoạn đó, chúng ta được phép chọn bất kỳ chuỗi con nào (không nhất thiết phải liền kề nhưng giữ nguyên thứ tự) để tạo thành một “mảng trò chơi”. 

Chuỗi con đã chọn đó được sử dụng trong trò chơi hai người chơi. Người chơi luân phiên di chuyển bắt đầu từ A. Trong một nước đi, người chơi chọn bất kỳ phần tử nào lớn hơn 1 và thay thế nó bằng một trong các ước số không tầm thường lớn hơn 1. Cuối cùng, các phần tử có thể bị giảm cho đến khi mọi thứ trở thành 1, tại thời điểm đó không thể di chuyển được và người chơi chuẩn bị di chuyển sẽ thua. 

Câu hỏi không phải là mô phỏng trò chơi một cách trực tiếp. Đối với mỗi phân đoạn, chúng ta phải đếm xem có bao nhiêu chuỗi con không trống khiến người chơi A giành chiến thắng trong cách chơi tối ưu và xuất ra số đếm này theo modulo 998244353. 

Các ràng buộc rất lớn, với tổng chiều dài mảng và tổng số truy vấn lên tới 10^6 trong các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ điều gì xây dựng hoặc đánh giá các chuỗi con cho mỗi truy vấn. Ngay cả O(n log n) cho mỗi truy vấn cũng sẽ quá chậm; chúng tôi cần O(1) hoặc O(log n) cho mỗi truy vấn sau khi xử lý trước. 

Một vấn đề tế nhị là các chuỗi con được xác định bằng các lựa chọn chỉ mục, vì vậy hai chuỗi con sẽ khác nhau nếu chúng chọn các vị trí khác nhau, ngay cả khi các giá trị trùng nhau. Ngoài ra, các chuỗi con trống không được phép. 

Trường hợp cạnh chính xuất hiện khi tất cả các phần tử được chọn đều bằng 1. Trong tình huống đó, không thể di chuyển được trong trò chơi, do đó người chơi đầu tiên sẽ thua ngay lập tức. Cách tiếp cận tính toán ngây thơ mà bỏ qua trường hợp này sẽ bao gồm các cấu hình chiến thắng không hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

Đối với mỗi truy vấn, giải pháp brute-force sẽ liệt kê tất cả các chuỗi con của phân đoạn và mô phỏng kết quả trò chơi. Ngay cả khi bỏ qua mô phỏng trò chơi, một đoạn có độ dài m có các chuỗi con 2^m và m có thể lớn, vì vậy điều này là không thể. 

Sự đơn giản hóa đầu tiên đến từ việc hiểu chính trò chơi. Mỗi bước di chuyển hoạt động trên một phần tử lớn hơn 1 và giảm phần tử đó bằng cách chọn một ước số. Bất kể số chia được chọn như thế nào, thực tế quan trọng duy nhất là phần tử giảm nghiêm ngặt và cuối cùng trở thành 1. Mỗi phần tử chỉ có thể được “sử dụng” cho đến khi nó trở thành 1, sau đó nó không liên quan. 

Điều này có nghĩa là mọi phần tử lớn hơn 1 đều đóng góp hiệu quả ít nhất một cơ hội di chuyển, trong khi các phần tử bằng 1 không đóng góp gì. Quan trọng hơn, vì người chơi luôn có thể giảm trực tiếp phần tử đã chọn xuống còn 1 trong một lần di chuyển, nên cấu trúc bên trong của hệ số hóa không ảnh hưởng đến cách chơi tối ưu: mỗi phần tử hữu ích hoạt động giống như một mã thông báo duy nhất có thể được sử dụng chính xác một lần. 

Vì vậy, trò chơi chuyển sang một quy trình theo lượt đơn giản: người chơi lần lượt tiêu thụ các phần tử lớn hơn 1. Người chiến thắng chỉ phụ thuộc vào số lượng phần tử như vậy tồn tại trong dãy con đã chọn. 

Nếu chúng ta xác định dãy con, gọi k là số phần tử lớn hơn 1 bên trong dãy đó. Trò chơi có đúng k nước đi, vì vậy A thắng khi và chỉ nếu k lẻ. 

Bây giờ bài toán trở thành tổ hợp thuần túy. Đối với một phân đoạn nhất định, chúng tôi đếm các chuỗi con sao cho số phần tử “tốt” được chọn (giá trị lớn hơn 1) là số lẻ. 

Giả sử có c phần tử lớn hơn 1 và z phần tử bằng 1 trong đoạn. Mỗi dãy con được hình thành bằng cách chọn độc lập từng phần tử hoặc không. Các phần tử z chỉ đóng góp vào sự tự do nhân lên, trong khi các phần tử c xác định tính chẵn lẻ. 

Trong số c phần tử, số tập con có kích thước lẻ chính xác là 2^(c-1) khi c > 0 và 0 khi c = 0. Mỗi lựa chọn phần tử z sẽ nhân đôi số chuỗi con hợp lệ, đóng góp hệ số 2^z. 

Vì vậy, với c > 0, câu trả lời trở thành 2^z * 2^(c-1) = 2^(độ dài đoạn - 1). Nếu c = 0, tất cả các phần tử đều bằng 1 và không tồn tại dãy con chiến thắng hợp lệ.

Do đó, mỗi truy vấn giảm xuống còn kiểm tra xem phân đoạn có chứa ít nhất một giá trị lớn hơn 1 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (các chuỗi + mô phỏng) | O(2^n · n) mỗi truy vấn | O(n) | Quá chậm | 
| Số tiền tố của giá trị > 1 | O(1) cho mỗi truy vấn sau khi xử lý trước O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Phân loại từng phần tử 

Chúng tôi quét mảng một lần và phân loại mọi phần tử là “hoạt động” nếu nó lớn hơn 1 hoặc “không hoạt động” nếu nó bằng 1. Chỉ các phần tử hoạt động mới ảnh hưởng đến tính chẵn lẻ của trò chơi. 

Sự phân loại này là đủ vì phép chia số chia bên trong không bao giờ ảnh hưởng đến thực tế là một phần tử vẫn có thể chơi được cho đến khi nó trở thành 1. 

### 2. Xây dựng thông tin tiền tố 

Chúng tôi tính toán một mảng tổng tiền tố trong đó mỗi vị trí lưu trữ số lượng phần tử hoạt động xuất hiện theo chỉ mục đó. Điều này cho phép chúng tôi truy vấn, đối với bất kỳ phân đoạn nào, nó chứa bao nhiêu phần tử hoạt động trong thời gian không đổi. 

Lý do điều này hiệu quả là vì các chuỗi con được chọn độc lập với các giá trị, vì vậy chúng ta chỉ cần số lượng chứ không cần vị trí. 

### 3. Trả lời từng truy vấn bằng cách sử dụng phân loại phân đoạn 

Đối với truy vấn [l, r], chúng tôi tính toán số phần tử hoạt động c trong phạm vi đó bằng cách sử dụng các khác biệt về tiền tố. Nếu c bằng 0 thì câu trả lời ngay lập tức là 0 vì trò chơi không có nước đi nào cả. 

Mặt khác, độ dài đoạn là len = r - l + 1. Mọi dãy con không trống đều có trọng số như nhau về mặt cấu trúc và chính xác một nửa số tập con trên các phần tử hoạt động có kích thước lẻ. Các phần tử không hoạt động chỉ nhân số đếm với 2^(số phần tử không hoạt động), vì vậy câu trả lời cuối cùng chỉ phụ thuộc vào len. 

Chúng tôi xuất ra 2^(len - 1) modulo 998244353. 

### Tại sao nó hoạt động 

Điều bất biến là chỉ có tính chẵn lẻ của số phần tử lớn hơn 1 trong dãy con đã chọn mới xác định được phần tử thắng cuộc. Phép chia số chia không bao giờ thay đổi cấu trúc liên quan đến tính chẵn lẻ này bởi vì mỗi phần tử hoạt động đóng góp chính xác một “đơn vị tiêu hao” không thể tránh khỏi trong cách chơi tối ưu. Vì các chuỗi con xử lý các phần tử một cách độc lập nên việc đếm giảm xuống số lượng tập hợp con bị ràng buộc bởi tính chẵn lẻ và các phần tử không hoạt động không ảnh hưởng đến tính chẵn lẻ mà chỉ chia tỷ lệ cho số lượng cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modexp(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    b = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (1 if b[i] > 1 else 0)

    for _ in range(q):
        l, r = map(int, input().split())
        c = pref[r] - pref[l - 1]
        if c == 0:
            print(0)
        else:
            length = r - l + 1
            print(modexp(2, length - 1))
```Mảng tiền tố`pref`theo dõi xem có bao nhiêu phần tử trong phân đoạn mảng lớn hơn 1. Mỗi truy vấn trừ hai giá trị tiền tố để nhận được số đếm trong O(1). 

Bước lũy thừa tính lũy thừa của hai modulo 998244353 một cách hiệu quả bằng cách sử dụng lũy ​​thừa nhị phân. Vì số mũ tối đa là 10^6 nên tốc độ này đủ nhanh trên tất cả các truy vấn. 

Một điểm tinh tế là chúng ta không bao giờ tính toán rõ ràng z, số lượng đơn vị, bởi vì nó bị loại bỏ trong biểu thức cuối cùng. Đây là điều khiến giải pháp có thể mở rộng quy mô cho các đầu vào lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét mảng:`[1, 1, 4, 16, 2, 8, 3, 7]`Lấy toàn bộ đoạn [1, 8]. 

Chúng tôi tính toán có bao nhiêu phần tử lớn hơn 1: 

| tôi | giá trị | > 1? | tiền tố | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 
| 3 | 4 | 1 | 1 | 
| 4 | 16 | 1 | 2 | 
| 5 | 2 | 1 | 3 | 
| 6 | 8 | 1 | 4 | 
| 7 | 3 | 1 | 5 | 
| 8 | 7 | 1 | 6 | 

Vậy c = 6, chiều dài = 8. Vì c > 0 nên đáp án là 2^(8 - 1) = 2^7 = 128. 

Điều này cho thấy rằng mặc dù việc phân bố số lượng lớn có vẻ phức tạp nhưng độ dài chỉ quan trọng khi tồn tại ít nhất một phần tử hoạt động. 

### Ví dụ 2 

Lấy một phân đoạn chỉ bao gồm những cái, giả sử`[1, 1, 1, 1]`. 

Ở đây c = 0. Không có phần tử nào có thể được sử dụng trong trò chơi, vì vậy A ngay lập tức thua ở mọi chuỗi tiếp theo. Câu trả lời là 0. 

Một cách tiếp cận đếm tập hợp con đơn giản bỏ qua điều kiện “không di chuyển” sẽ đưa ra sai 2^4 - 1 = 15, nhưng những chuỗi con đó không thỏa mãn điều kiện trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Tính toán tiền tố là tuyến tính, mỗi truy vấn là O(1) cộng với lũy thừa mô-đun theo thời gian không đổi | 
| Không gian | O(n) | Lưu trữ mảng tiền tố | 

Tổng số ràng buộc đối với tất cả các trường hợp thử nghiệm đạt tới 10^6, vì vậy việc xử lý trước tuyến tính và truy vấn thời gian không đổi là rất cần thiết. Giải pháp vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modexp(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, q = map(int, input().split())
        b = list(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + (1 if b[i] > 1 else 0)

        for _ in range(q):
            l, r = map(int, input().split())
            c = pref[r] - pref[l - 1]
            if c == 0:
                out.append("0")
            else:
                out.append(str(modexp(2, r - l)))
    return "\n".join(out)

# custom tests

assert solve("""1
1 1
1
1 1
""") == "0"

assert solve("""1
3 1
2 3 4
1 3
""") == str(modexp(2, 2))

assert solve("""1
4 1
1 1 1 1
1 4
""") == "0"

assert solve("""1
5 2
1 2 1 3 1
1 5
2 4
""") == "\n".join([str(modexp(2,4)), str(modexp(2,2))])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 0 | tất cả các yếu tố đã chết | 
| tất cả đều hoạt động | 2^(n-1) | hành vi đầy đủ của phân khúc hoạt động | 
| tất cả những cái | 0 | hiệu lực trò chơi trống | 
| truy vấn hỗn hợp | sức mạnh của hai | tính nhất quán trên các phạm vi | 

## Vỏ cạnh 

Khi một phân đoạn chỉ chứa một phân đoạn, thuật toán sẽ trả về 0 một cách chính xác vì chênh lệch tiền tố c trở thành 0, kích hoạt trường hợp đặc biệt. Điều này tránh việc đếm sai các chuỗi con không có nước đi thực sự trong trò chơi. 

Khi một phân đoạn chứa chính xác một phần tử lớn hơn 1, mọi chuỗi con hợp lệ bao gồm nó sẽ đóng góp một điều kiện thắng hoàn toàn dựa trên tính chẵn lẻ bao gồm và công thức 2^(len-1) vẫn giữ nguyên. Việc kiểm tra tiền tố đảm bảo chúng tôi không coi các phân đoạn toàn bộ là hợp lệ một cách nhầm lẫn. 

Khi phân đoạn lớn nhưng thưa thớt các phần tử hoạt động, thuật toán vẫn hoạt động chính xác vì nó không bao giờ phụ thuộc vào vị trí của các phần tử hoạt động mà chỉ phụ thuộc vào sự tồn tại của chúng.
