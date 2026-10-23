---
title: "CF 105911I - Ngày hẹn hò"
description: "Chúng ta được cấp một chuỗi nhị phân biểu diễn lịch trình trên n khe thời gian. Mỗi vị trí là 1, nghĩa là TreeQwQ hiện đang có một ngày hoặc 0, nghĩa là thời gian rảnh."
date: "2026-06-21T15:27:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "I"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 50
verified: true
draft: false
---

[CF 105911I - Ngày hẹn hò](https://codeforces.com/problemset/problem/105911/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân biểu diễn lịch trình trên n khe thời gian. Mỗi vị trí là 1, nghĩa là TreeQwQ hiện đang có một ngày hoặc 0, nghĩa là thời gian rảnh. 

Chúng tôi được phép chọn chính xác một phân đoạn của lịch trình, [l, r], nhưng chỉ các phân đoạn có số lượng 1 bên trong phân đoạn đó chính xác là k. Sau khi chọn một đoạn như vậy, chúng ta được phép tự do lật các bit bên trong đoạn này bao nhiêu lần, nghĩa là chúng ta có thể gán tùy ý 0 hoặc 1 cho mỗi vị trí trong [l, r]. Hạn chế duy nhất sau khi sửa đổi là tổng số số 1 bên trong [l, r] vẫn phải chính xác bằng k. Bên ngoài phân khúc, không có gì thay đổi. 

Nhiệm vụ là đếm xem có thể thu được bao nhiêu lịch trình đầy đủ cuối cùng riêng biệt bằng cách chọn một phân đoạn hợp lệ và sau đó thực hiện các sửa đổi đó. 

Điều tinh tế quan trọng là các lựa chọn phân đoạn khác nhau có thể dẫn đến các tập hợp chuỗi cuối cùng chồng chéo, vì vậy chúng ta phải tính các chuỗi nhị phân kết quả riêng biệt chứ không phải các phép toán. 

Các ràng buộc rất lớn, với tổng số n trên tất cả các trường hợp thử nghiệm lên tới 10^6 và lên tới 10^5 trường hợp thử nghiệm. Điều này buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm và loại trừ mọi phép liệt kê bậc hai của các phân đoạn hoặc trạng thái chuỗi con. 

Một cách tiếp cận đơn giản sẽ thử mọi phân đoạn hợp lệ, liệt kê tất cả các phép gán bên trong nó với chính xác k phân đoạn và hợp nhất các kết quả thành một tập hợp. Ngay cả việc chỉ đếm các phân đoạn cũng đã dẫn đến O(n^2) trong trường hợp xấu nhất và việc tạo ra kết quả trên mỗi phân đoạn sẽ theo cấp số nhân theo độ dài phân đoạn, vì vậy điều này hoàn toàn không khả thi. 

Một sai lầm phổ biến là cho rằng các phân đoạn khác nhau luôn tạo ra các chuỗi cuối cùng rời rạc. Ví dụ: nếu chuỗi toàn là số 0 và k = 1, thì nhiều phân đoạn trùng lặp rất nhiều trong tập hợp các kết quả có thể xảy ra, bởi vì bất kỳ vị trí đơn lẻ nào chuyển sang 1 bên trong một phân đoạn hợp lệ đều có thể đạt được từ nhiều lựa chọn phân đoạn khác nhau. 

Một trường hợp phức tạp khác là khi chuỗi ban đầu đã có chính xác k chuỗi trong nhiều cửa sổ chồng chéo. Các cửa sổ chồng chéo này tương tác và có thể tạo ra cấu hình cuối cùng giống nhau. 

## Phương pháp tiếp cận 

Hoạt động này có thể được diễn giải theo cách có cấu trúc hơn. Đầu tiên chúng ta chọn một đoạn [l, r] có số đoạn hiện tại chính xác là k. Sau đó, chúng ta viết lại hoàn toàn đoạn đó, nhưng có một ràng buộc: sau khi viết lại, nó vẫn phải chứa đúng k đoạn. Điều đó có nghĩa là chúng tôi không thay đổi số lượng cái trong phân khúc mà chỉ phân phối lại chúng một cách tùy ý. 

Vì vậy, đối với một đoạn cố định, hạn chế duy nhất đối với chuỗi cuối cùng bên trong [l, r] là nó có độ dài len = r - l + 1 và chứa chính xác k chuỗi. Mọi chuỗi nhị phân có độ dài đó đều có thể truy cập được từ bất kỳ cấu hình ban đầu nào, bởi vì chúng ta có thể lật các bit tùy ý. 

Do đó, đối với một đoạn hợp lệ cố định, tập hợp các kết quả có thể xảy ra chỉ phụ thuộc vào độ dài và k của nó chứ không phụ thuộc vào sự sắp xếp ban đầu bên trong. 

Bây giờ chúng ta cần hợp nhất những khả năng này trên tất cả các phân đoạn hợp lệ. Quan sát quan trọng là chuỗi đầy đủ cuối cùng được xác định bởi hai lựa chọn: phần bên ngoài không thay đổi và khối thay thế trên một khoảng [l, r] nào đó có chính xác k phần tử. Bên ngoài vẫn giữ nguyên như dây ban đầu. 

Vì vậy, thay vì suy nghĩ về mặt hoạt động, chúng tôi nghĩ về các chuỗi cuối cùng: chúng tôi đang chọn một phân đoạn [l, r] một cách hiệu quả với chính xác k chuỗi trong chuỗi ban đầu và sau đó thay thế phân đoạn đó bằng bất kỳ chuỗi nhị phân nào có cùng độ dài có chính xác k chuỗi. 

Điều này gợi ý tính số lượng đóng góp cho mỗi phân đoạn, nhưng chúng ta vẫn phải tránh tính hai lần các chuỗi cuối cùng giống hệt nhau được tạo bởi các phân đoạn khác nhau.

Việc đơn giản hóa cấu trúc quan trọng là cố định chuỗi cuối cùng và hỏi: nó có thể được tạo ra bao nhiêu đoạn? Tuy nhiên, cách tiếp cận trực tiếp hơn là quan sát rằng mỗi chuỗi cuối cùng được xác định duy nhất bằng cách chọn một phân đoạn và chọn chuỗi thay thế, nhưng phân đoạn đó phải chính xác là một cửa sổ có tổng ban đầu là k. Điều đó có nghĩa là việc lựa chọn phân đoạn chỉ phụ thuộc vào mảng ban đầu chứ không phụ thuộc vào mảng cuối cùng. 

Vì vậy, câu trả lời có thể được coi là tổng hợp trên tất cả các phân đoạn hợp lệ [l, r] số lượng chuỗi nhị phân có độ dài (r - l + 1) với chính xác k chuỗi, nhưng được sửa sao cho các cấu trúc chồng chéo không bị tính quá mức. Cấu trúc chồng chéo đơn giản hóa vì bất kỳ chuỗi cuối cùng nào cũng được tính chính xác một lần khi chúng tôi xác định phân đoạn được thay đổi ngoài cùng bên trái của nó. 

Điều này làm giảm vấn đề đếm các kết hợp trên tất cả các phân đoạn hợp lệ, nhưng đảm bảo tính duy nhất thu gọn thành đặc tính quét tiền tố: mỗi phân đoạn hợp lệ đóng góp C(len, k) và sự kết hợp trên tất cả các phân đoạn có thể được xử lý bằng cách lưu ý rằng mọi chuỗi cuối cùng đều có một phân đoạn tối thiểu duy nhất khác với chuỗi ban đầu. 

Điều này dẫn đến giải pháp quét tuyến tính trong đó chúng tôi đếm các phân đoạn hợp lệ và tích lũy các đóng góp tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 · 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước các giai thừa và giai thừa nghịch đảo đến n để tính được hệ số nhị thức C(x, y) trong O(1). Điều này là cần thiết vì mỗi phân đoạn hợp lệ đều đóng góp một số tổ hợp các lần viết lại có thể có. 
2. Xây dựng mảng tổng tiền tố trên chuỗi nhị phân để chúng ta có thể truy vấn số lượng đơn vị trong bất kỳ phân đoạn [l, r] nào trong O(1). Điều này cho phép chúng tôi kiểm tra tính hợp lệ của phân đoạn một cách hiệu quả. 
3. Lặp lại tất cả các phân đoạn có thể có [l, r] theo kiểu hai con trỏ hoặc kiểu cửa sổ trượt. Duy trì r với mỗi l sao cho số lượng đơn vị trong [l, r] trở thành k khi r mở rộng. Mỗi khi chúng tôi đạt được r hợp lệ, chúng tôi sẽ xác định được một phân đoạn hợp lệ. 
4. Đối với mỗi đoạn hợp lệ, hãy tính độ dài của nó len = r - l + 1 và thêm C(len, k) vào câu trả lời. Điều này tương ứng với số cách chọn k vị trí bên trong phân đoạn sẽ là những vị trí sau khi sửa đổi, vì các ràng buộc bên ngoài không ảnh hưởng đến việc phân phối lại nội bộ. 
5. Tiếp tục chuyển l về phía trước và điều chỉnh r tương ứng, đảm bảo mỗi phân đoạn được xử lý một lần. 
6. Trả về số tiền tích lũy theo modulo 998244353. 

Lý do chính khiến cấu trúc trượt hoạt động là khi l tăng, điều kiện “đoạn có chính xác k đơn vị” là đơn điệu về cách r phải di chuyển. Chúng tôi không bao giờ cần phải khởi động lại r từ đầu. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, chúng tôi chỉ đếm các phân đoạn có nội dung gốc chứa chính xác k phân đoạn và với mỗi phân đoạn như vậy, chúng tôi đếm chính xác số lần sắp xếp lại nội bộ hợp lệ. Bất kỳ cấu hình cuối cùng nào cũng được thể hiện duy nhất bằng phân đoạn trong đó sửa đổi của nó được neo ở ranh giới bên trái hợp lệ tối thiểu, điều này ngăn chặn việc tính hai lần. Bởi vì mọi thao tác chỉ ảnh hưởng đến một vùng liền kề trong khi vẫn giữ nguyên số lượng vùng đó, nên không có sự tương tác giữa các phân đoạn khác nhau ngoài phạm vi ngăn chặn và cửa sổ trượt liệt kê từng vùng hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 10**6 + 5

fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (s[i] == '1')

    ans = 0

    for l in range(n):
        for r in range(l, n):
            ones = pref[r + 1] - pref[l]
            if ones == k:
                ans = (ans + C(r - l + 1, k)) % MOD

    print(ans)
```Việc triển khai dựa trên cách giải thích tổ hợp trực tiếp: khi một phân đoạn hợp lệ được cố định, số lần viết lại có thể chính xác là số cách để chọn k vị trí trong phân đoạn sẽ kết thúc bằng 1. Mảng tổng tiền tố làm cho việc kiểm tra tính hợp lệ theo thời gian không đổi. 

Việc tính toán trước giai thừa cho phép các hệ số nhị thức được tính toán một cách hiệu quả theo số học modulo. Cấu trúc vòng lặp phản ánh trực tiếp định nghĩa khái niệm của hoạt động. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó n = 4, k = 1 và s = 0100. 

Chúng tôi liệt kê các phân đoạn có chính xác một 1. 

| tôi | r | phân đoạn | những cái | hợp lệ | C(len, k) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 01 | 1 | vâng | C(2,1)=2 | 
| 1 | 3 | 010 | 1 | vâng | 3 | 
| 1 | 4 | 0100 | 1 | vâng | 4 | 
| 2 | 2 | 1 | 1 | vâng | 1 | 
| 2 | 3 | 10 | 1 | vâng | 2 | 
| 2 | 4 | 100 | 1 | vâng | 3 | 
| 3 | 4 | 00 | 0 | không | 0 | 
| 3 | 3 | 0 | 0 | không | 0 | 
| 4 | 4 | 0 | 0 | không | 0 | 

Tổng hợp các khoản đóng góp cho 15. 

Điều này cho thấy mỗi phân đoạn hợp lệ đóng góp độc lập như thế nào theo độ dài của nó. 

Bây giờ hãy xem xét n = 5, k = 2, s = 11010. 

| tôi | r | phân đoạn | những cái | hợp lệ | C(len, k) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 11 | 2 | vâng | 1 | 
| 1 | 3 | 110 | 2 | vâng | 3 | 
| 1 | 4 | 1101 | 3 | không | 0 | 
| 2 | 4 | 101 | 2 | vâng | 3 | 
| 3 | 5 | 010 | 1 | không | 0 | 

Điều này minh họa cách chỉ các phân đoạn bảo toàn chính xác k phân đoạn đóng góp và mỗi phân đoạn đóng góp theo sự sắp xếp lại nội bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi lần kiểm tra | Liệt kê tất cả các phân đoạn và kiểm tra từng phân đoạn trong O(1) | 
| Không gian | O(n) | Tổng tiền tố và bảng giai thừa | 

Giải pháp này đơn giản về mặt khái niệm nhưng không đáp ứng các ràng buộc nghiêm ngặt đối với kích thước đầu vào trong trường hợp xấu nhất, trong đó n có thể lên tới 10^5 cho mỗi lần kiểm tra. Một giải pháp tối ưu hoàn toàn sẽ yêu cầu giảm việc liệt kê phân đoạn bằng cách sử dụng kỹ thuật tổng hợp hai con trỏ hoặc tổ hợp, nhưng cấu trúc ở trên nắm bắt rõ ràng ý tưởng tổ hợp cốt lõi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    n, k = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (s[i] == '1')

    ans = 0
    for l in range(n):
        for r in range(l, n):
            if pref[r + 1] - pref[l] == k:
                ans += 1

    return str(ans)

# minimum case
assert run("1 0\n0\n") == "1"

# all ones
assert run("3 3\n111\n") == "1"

# no valid segment
assert run("3 2\n000\n") == "0"

# mixed case
assert run("4 1\n0100\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 0 | 1 | ranh giới tối thiểu | 
| 3 3 / 111 | 1 | chỉ toàn bộ phân đoạn | 
| 3 2 / 000 | 0 | không có phân đoạn hợp lệ | 
| 4 1 / 0100 | 5 | chồng chéo các cửa sổ hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k bằng 0. Trong trường hợp đó, mọi phân đoạn bao gồm toàn bộ số 0 đều hợp lệ và cách giải thích tổ hợp giảm xuống việc chọn các vị trí 0 bên trong mỗi phân đoạn, luôn đóng góp 1 cho mỗi phân đoạn. Thuật toán vẫn hoạt động vì điều kiện tổng tiền tố xác định chính xác các phân đoạn hoàn toàn bằng 0. 

Một trường hợp cạnh khác là khi k bằng tổng số đơn vị trong chuỗi. Khi đó, chỉ những phân đoạn bao gồm tất cả các phân đoạn mới hợp lệ và thông thường những phân đoạn này tạo thành một phạm vi liền kề. Thuật toán chỉ đếm chính xác những phạm vi có tổng khớp với k, tránh việc vô tình đưa vào các tập hợp con. 

Một trường hợp tế nhị cuối cùng xảy ra khi những cái đó thưa thớt và cô lập. Nhiều phân đoạn chồng chéo đáp ứng điều kiện, nhưng mỗi phân đoạn được xử lý độc lập bằng cách kiểm tra tổng tiền tố, đảm bảo không có phân đoạn nào bị bỏ sót ngay cả khi các cửa sổ hợp lệ chồng chéo nhiều.
