---
title: "CF 105761E - Nhóm hướng dẫn"
description: "Chúng tôi được cung cấp một danh sách học sinh, mỗi học sinh có một trình độ kiến ​​thức khác nhau. Mục đích là phân chia chúng thành các nhóm liền kề sau khi sắp xếp theo cấp độ kiến ​​thức, sao cho mỗi nhóm thỏa mãn hai ràng buộc: phạm vi bên trong nhóm, được định nghĩa là kiến ​​thức tối đa trừ kiến ​​thức tối thiểu…"
date: "2026-06-21T22:54:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "E"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 55
verified: true
draft: false
---

[CF 105761E - Nhóm hướng dẫn](https://codeforces.com/problemset/problem/105761/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách học sinh, mỗi học sinh có một trình độ kiến thức khác nhau. Mục tiêu là phân chia chúng thành các nhóm liền kề sau khi sắp xếp theo cấp độ kiến ​​thức, sao cho mỗi nhóm thỏa mãn hai ràng buộc: phạm vi bên trong nhóm, được xác định là kiến ​​thức tối đa trừ kiến ​​thức tối thiểu, không vượt quá giá trị cố định k và kích thước nhóm không vượt quá s. 

Sau khi các nhóm được hình thành, chúng sẽ được sắp xếp theo thứ tự bằng cách tự động tăng phạm vi kiến ​​thức vì chúng ta đang làm việc trên các giá trị được sắp xếp. Phân vùng hợp lệ là bất kỳ cách nào để chia mảng đã sắp xếp thành các phân đoạn liên tiếp sao cho mỗi phân đoạn tuân theo cả hai ràng buộc. Mỗi phân đoạn khác nhau được tính là một giải pháp khác nhau và chúng ta phải tính tất cả chúng theo modulo 1e9 + 7. 

Kích thước đầu vào n có thể lên tới 10000, loại trừ việc liệt kê theo cấp số nhân trên tất cả các phân vùng hoặc tất cả các tập hợp con. Ngay cả O(n^2) cũng khó có thể vượt qua nếu được tối ưu hóa, nhưng bất kỳ thứ gì có tính khối hoặc tệ hơn đều không thể thực hiện được ngay lập tức. Ràng buộc bổ sung s ≤ 100 là giới hạn cấu trúc quan trọng sẽ định hình giải pháp. 

Một điểm tinh tế là việc phân nhóm hoàn toàn là vị trí sau khi sắp xếp, vì vậy chúng tôi không chọn các tập hợp con tùy ý, chỉ chọn các phân vùng của một mảng được sắp xếp. Một điều tinh tế khác là mặc dù k có thể lớn (lên tới 1e9), nhưng nó chỉ ảnh hưởng đến việc một phân đoạn có hợp lệ hay không chứ không ảnh hưởng trực tiếp đến cấu trúc tổ hợp. 

Một sai lầm ngây thơ là coi đây là “chọn các vết cắt ở bất kỳ đâu một cách độc lập”. Điều đó không thành công vì tính hợp lệ của một phân đoạn phụ thuộc vào cả kích thước và phạm vi tối thiểu/tối đa, do đó việc cắt có được phép hay không phụ thuộc vào cửa sổ bên trái của nó. 

Đối với trường hợp cạnh bê tông, xét n = 3, s = 2, k = 0 với các giá trị [1, 2, 3]. Không có nhóm kích thước 2 nào hợp lệ vì bất kỳ cặp nào cũng có phạm vi 1 > 0, do đó phân vùng hợp lệ duy nhất là {1}, {2}, {3}. Cách tiếp cận ngây thơ bỏ qua k có thể đếm không chính xác các phân vùng như {1,2},{3}. 

Một trường hợp cạnh khác là khi k cực lớn, nghĩa là chỉ có ràng buộc về kích thước mới quan trọng. Sau đó, vấn đề trở thành việc đếm các phân vùng trong đó mỗi phân đoạn có độ dài tối đa là s, đó là DP tiêu chuẩn với các chuyển đổi cửa sổ trượt. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc, chúng ta có thể cố gắng tạo mọi phân vùng có thể có của mảng đã được sắp xếp và kiểm tra tính hợp lệ của từng nhóm. Điều này sẽ liên quan đến việc khám phá mọi cấu hình cắt, có khả năng 2^(n-1). Ngay cả với n = 30 thì điều này vẫn lớn và ở n = 10000 thì điều đó hoàn toàn không thể xảy ra. 

Thay vào đó chúng ta có thể suy nghĩ theo thuật ngữ lập trình động. Đặt dp[i] biểu thị số cách hợp lệ để phân vùng tiền tố kết thúc tại chỉ mục i trong mảng được sắp xếp. Nếu nhóm cuối cùng bắt đầu ở j và kết thúc ở i thì chúng ta cần đoạn [j, i] đó thỏa mãn cả hai ràng buộc. Nếu có, dp[i] có thể thêm dp[j-1]. 

Khó khăn là tìm ra tất cả j hợp lệ cho mỗi i một cách hiệu quả. Một lần quét đơn giản sẽ kiểm tra các vị trí trước đó, cho ra O(n * s) là đường biên nhưng thực tế khả thi với s ≤ 100. Tuy nhiên, về mặt khái niệm, chúng ta có thể làm tốt hơn bằng cách lưu ý rằng tính hợp lệ bị chi phối bởi hai điều kiện trượt: giới hạn kích thước ngay lập tức giới hạn j ≥ i - s + 1 và giới hạn ràng buộc phạm vi j sao cho a[i] - a[j] ≤ k. Vì mảng đã được sắp xếp nên với mỗi i chúng ta có thể tìm thấy j hợp lệ ngoài cùng bên trái bằng kỹ thuật hai con trỏ. 

Điều này biến đổi các chuyển đổi thành tổng phạm vi giới hạn trên dp, có thể được tính bằng tổng tiền tố. Điều đó loại bỏ hoàn toàn vòng lặp bên trong, giảm vấn đề xuống O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP với tính năng quét cửa sổ | O(n · s) | O(n) | Chấp nhận được nhưng chặt chẽ | 
| DP với hai con trỏ + tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Đầu tiên chúng ta sắp xếp mảng vì việc nhóm chỉ phụ thuộc vào thứ tự tương đối của các cấp độ kiến ​​thức. Bất kỳ nhóm hợp lệ nào cũng phải tôn trọng thứ tự này, do đó việc sắp xếp sẽ chuyển vấn đề thành vấn đề phân vùng 1D. 

Chúng ta định nghĩa dp[i] là số cách hợp lệ để phân chia i sinh viên đầu tiên sau khi sắp xếp. 

Chúng ta cũng duy trì một con trỏ L để theo dõi chỉ mục sớm nhất sao cho đoạn [L, i] thỏa mãn ràng buộc a[i] - a[L] ≤ k. Vì tôi chỉ tiến về phía trước nên L cũng chỉ có thể tiến về phía trước. 

Ngoài ra, chúng tôi thực thi ràng buộc kích thước bằng cách đảm bảo các phân đoạn có kích thước tối đa là s, nghĩa là ranh giới bên trái ít nhất phải là i - s + 1. 

Đối với mỗi i, chúng tôi tính toán phạm vi hợp lệ của các vị trí bắt đầu j cho nhóm cuối cùng. Phạm vi đó là từ L đến R trong đó R = i - 1 và L được điều chỉnh để đáp ứng cả hai ràng buộc. Khi đó chúng ta cần dp[i] = tổng của dp[j] với mọi j trong [L, R]. 

Để tính tổng này một cách hiệu quả, chúng tôi duy trì một mảng tổng tiền tố trên dp. 

### Các bước 

1. Sắp xếp mảng các cấp độ kiến thức. Điều này đảm bảo rằng bất kỳ nhóm hợp lệ nào cũng tương ứng với một phân đoạn liền kề theo thứ tự được sắp xếp. Nếu không sắp xếp, các ràng buộc về phạm vi sẽ không chuyển thành các khoảng liền kề. 
2. Khởi tạo dp[0] = 1, biểu thị tiền tố trống có một phân vùng hợp lệ. 
3. Duy trì con trỏ L = 0 để theo dõi điểm bắt đầu hợp lệ ngoài cùng bên trái cho vị trí kết thúc hiện tại. 
4. Với mỗi vị trí i từ 1 đến n, di chuyển L về phía trước trong khi a[i] - a[L] > k. Điều này đảm bảo tất cả các phân đoạn bắt đầu trước L đều không hợp lệ do vi phạm phạm vi. 
5. Tính ranh giới giới hạn kích thước là i - s + 1 và cập nhật L = max(L, i - s + 1). Điều này thực thi hạn chế kích thước nhóm tối đa. 
6. Bây giờ, các vị trí cắt hợp lệ trước đó j nằm trong [L - 1, i - 1] theo thuật ngữ lập chỉ mục dp. Sự thay đổi này xuất phát từ việc dp dựa trên tiền tố 1. 
7. Sử dụng tổng tiền tố để tính dp[i] là tổng của dp[j] trên phạm vi đó trong thời gian O(1). 
8. Cập nhật tổng tiền tố sau mỗi lần tính toán dp[i] để các trạng thái trong tương lai có thể được truy vấn một cách hiệu quả. 

### Tại sao nó hoạt động 

Tại bất kỳ chỉ số i nào, mọi nhóm cuối cùng hợp lệ phải kết thúc tại i và bắt đầu tại một số j thỏa mãn cả hai ràng buộc. Con trỏ trượt đảm bảo rằng tất cả các lần khởi động không hợp lệ sẽ bị loại trừ chính xác một lần, không bao giờ loại bỏ sớm các lần khởi động hợp lệ. Tổng tiền tố tổng hợp tất cả các vị trí cắt cuối cùng có thể có, nghĩa là mỗi phân vùng hợp lệ được tính chính xác một lần theo điểm phân chia cuối cùng của nó. Bất biến DP là dp[i] tính tất cả các phân vùng hợp lệ của tiền tố [1..i] và mỗi phân vùng như vậy được xác định duy nhất bởi vị trí của lần cắt cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k, s = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

dp = [0] * (n + 1)
pref = [0] * (n + 1)

dp[0] = 1
pref[0] = 1

L = 0

for i in range(1, n + 1):
    while L < i and a[i - 1] - a[L] > k:
        L += 1

    if i - L > s:
        L = i - s

    left = L
    right = i - 1

    if left <= right:
        dp[i] = (pref[right] - pref[left - 1]) % MOD
    else:
        dp[i] = 0

    pref[i] = (pref[i - 1] + dp[i]) % MOD

print(dp[n])
```Giải pháp được xây dựng dựa trên DP qua tiền tố, trong đó mỗi trạng thái tổng hợp tất cả các phân vùng hợp lệ kết thúc tại một chỉ mục nhất định. Mảng được sắp xếp đảm bảo tính hợp lệ có thể được kiểm tra bằng cách sử dụng các khoảng thời gian và con trỏ trượt đảm bảo chúng tôi chỉ xem xét các lần khởi động khả thi. 

Mảng tổng tiền tố rất quan trọng vì nó chuyển đổi những gì lẽ ra sẽ là quá trình chuyển đổi O(s) trên mỗi trạng thái thành O(1). Việc xử lý phép trừ sử dụng số học mô-đun một cách cẩn thận để tránh các giá trị âm. 

Một chi tiết tinh tế là căn chỉnh các chỉ số: dp dựa trên 1 dựa trên độ dài tiền tố trong khi mảng dựa trên 0, vì vậy hãy cẩn thận khi chuyển đổi giữa chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, k = 5, s = 3 

a = [5, 6, 9, 10, 12] 

Mảng được sắp xếp không thay đổi. 

Chúng tôi theo dõi dp và L: 

| tôi | một [tôi] | L (ràng buộc phạm vi) | hạn chế kích thước L | L cuối cùng | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 0 | 0 | 1 | 
| 2 | 6 | 0 | 0 | 0 | 2 | 
| 3 | 9 | 0 | 0 | 0 | 4 | 
| 4 | 10 | 0 | 1 | 1 | 7 | 
| 5 | 12 | 1 | 2 | 2 | 13 | 

Tại i = 4, ràng buộc kích thước bắt đầu hạn chế các lần khởi động cũ hơn, dịch chuyển L về phía trước. Điều này thể hiện cách cả hai ràng buộc tương tác với nhau: ràng buộc phạm vi không hoạt động, ràng buộc kích thước chiếm ưu thế sau này. 

Câu trả lời cuối cùng là dp[5] = 13. 

### Ví dụ 2 

đầu vào: 

n = 4, k = 0, s = 2 

a = [1, 2, 3, 4] 

Ở đây chỉ có các nhóm phần tử đơn là hợp lệ. 

| tôi | hợp lệ L | dp[i] | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 1 | 
| 3 | 2 | 1 | 
| 4 | 3 | 1 | 

Mỗi phần tử phải tạo thành nhóm riêng nên chỉ tồn tại một phân vùng. 

Điều này xác nhận thuật toán thực thi chính xác ràng buộc phạm vi ngay cả khi kích thước sẽ cho phép nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc sắp xếp chiếm ưu thế ở O(n log n), chuyển tiếp DP là O(1) trên mỗi chỉ mục bằng cách sử dụng tổng tiền tố và một con trỏ đơn điệu | 
| Không gian | O(n) | mảng dp và tiền tố lưu trữ giá trị cho tất cả các tiền tố | 

Các ràng buộc n 10000 làm cho việc sắp xếp O(n log n) trở nên tầm thường và DP tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng ít vì chúng tôi chỉ lưu trữ một vài mảng có kích thước n. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k, s = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    dp = [0] * (n + 1)
    pref = [0] * (n + 1)

    dp[0] = 1
    pref[0] = 1

    L = 0

    for i in range(1, n + 1):
        while L < i and a[i - 1] - a[L] > k:
            L += 1

        if i - L > s:
            L = i - s

        left = L
        right = i - 1

        if left <= right:
            dp[i] = (pref[right] - (pref[left - 1] if left > 0 else 0)) % MOD
        else:
            dp[i] = 0

        pref[i] = (pref[i - 1] + dp[i]) % MOD

    return str(dp[n])

# provided sample-like tests
assert run("5 5 3\n10 6 5 9 12\n") == "13"

# minimum size
assert run("1 10 1\n7\n") == "1"

# all equal-ish spacing but k tight
assert run("3 0 2\n1 2 3\n") == "1"

# large k, small s
assert run("5 1000 2\n1 2 3 4 5\n") == "8"

# s = 1 forces singleton partitions
assert run("4 10 1\n4 1 3 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | tính đúng đắn của trường hợp cơ sở | 
| chuỗi k = 0 | 1 | hạn chế phạm vi nghiêm ngặt | 
| k lớn, s=2 | nhiều phân vùng | hành vi cửa sổ trượt | 
| s = 1 | 1 | thực thi hạn chế kích thước | 

## Vỏ cạnh 

Trường hợp một cạnh là khi k cực kỳ nhỏ, buộc tất cả các nhóm trở thành đơn lẻ một cách hiệu quả. Thuật toán xử lý điều này vì con trỏ L sẽ luôn tiến lên để loại trừ bất kỳ phân đoạn nào có phạm vi dương, chỉ để lại các chuyển đổi dp từ chỉ mục ngay trước đó. 

Một trường hợp cạnh khác là khi s = 1, buộc mỗi nhóm phải chứa chính xác một phần tử. Trong trường hợp này, phạm vi hợp lệ cho mỗi i thu gọn lại chỉ còn i chính nó khi bắt đầu, do đó các chuyển tiếp dp[i] = dp[i-1] biến mất và dp trở thành một tiến trình không đổi 1. 

Trường hợp thứ ba là khi k cực lớn, làm cho giới hạn phạm vi không còn phù hợp. Khi đó L không bao giờ di chuyển do sự khác biệt về giá trị và chỉ giới hạn kích thước i - s + 1 mới giới hạn chuyển đổi. Thuật toán giảm xuống việc đếm các thành phần có kích thước khối giới hạn và tổng tiền tố sẽ tổng hợp chính xác các chuyển đổi đó mà không cần sửa đổi.
