---
title: "CF 104755H - Hình tam giác"
description: "Cho ta một tập hợp các đường thẳng trong mặt phẳng. Mỗi đường được đảm bảo là một trong ba hướng đặc biệt: đường ngang có dạng $y = c$, đường thẳng đứng có dạng $x = c$ và đường chéo có dạng $x + y = c$."
date: "2026-06-28T22:53:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104755
codeforces_index: "H"
codeforces_contest_name: "LU ICPC Selection Contest 2023"
rating: 0
weight: 104755
solve_time_s: 47
verified: true
draft: false
---

[CF 104755H - Hình tam giác](https://codeforces.com/problemset/problem/104755/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một tập hợp các đường thẳng trong mặt phẳng. Mỗi dòng được đảm bảo là một trong ba hướng đặc biệt: các đường ngang của biểu mẫu$y = c$, các đường thẳng đứng có dạng$x = c$, và các đường chéo có dạng$x + y = c$. Tất cả các hằng số$c$là số nguyên và tất cả các dòng đều khác nhau. 

Nhiệm vụ là đếm xem có thể tạo được bao nhiêu tam giác không suy biến sao cho mỗi cạnh của tam giác nằm hoàn toàn trên một trong các đường thẳng cho trước. Một tam giác hợp lệ được xác định bằng cách chọn ba đường thẳng có giao điểm theo cặp tạo thành ba điểm phân biệt, sao cho các điểm đó tạo thành một tam giác đúng chứ không phải là một cấu hình suy biến trong đó cả ba đường thẳng gặp nhau tại một hoặc hai điểm song song. 

Ràng buộc cấu trúc chính là các cạnh bị hạn chế nằm chính xác trên các đường cho trước, có nghĩa là mỗi tam giác tương ứng với việc chọn ba đường, mỗi đường một cạnh, với yêu cầu ba đường này cắt nhau theo cặp ở ba điểm khác nhau. 

Sự ràng buộc$n \le 3000$ngay lập tức loại trừ bất kỳ$O(n^3)$liệt kê bộ ba dòng. Thậm chí$O(n^2)$là đường biên nhưng khả thi nếu mỗi cặp được xử lý trong thời gian không đổi. Cấu trúc của các họ đường gợi ý rõ ràng rằng chúng ta nên tránh lý luận về các giao điểm tùy tiện mà thay vào đó hãy khai thác thực tế là chỉ tồn tại ba hướng. 

Một vấn đề tế nhị là sự thoái hóa do sự đồng thời gây ra. Ba đường có thể gặp nhau tại một điểm, điều này xảy ra chẳng hạn khi đường ngang, đường dọc và đường chéo có chung một giao điểm. Một trường hợp thất bại khác là chọn hai đường thẳng cùng hướng, song song và không thể tạo thành một cặp cạnh tam giác. 

Ví dụ, nếu chúng ta lấy$y = 0$,$x = 0$, Và$x + y = 0$, cả ba đường thẳng gặp nhau tại gốc tọa độ, vì vậy mặc dù chúng ta có ba đường thẳng hợp lệ nhưng chúng không tạo thành một hình tam giác. Một cách tiếp cận đếm đơn giản chỉ kiểm tra các giao điểm theo cặp mà không thực thi các đỉnh riêng biệt sẽ vượt quá các cấu hình như vậy. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ trực tiếp sẽ lặp lại trên tất cả các bộ ba dòng và kiểm tra xem chúng có tạo thành một tam giác hợp lệ hay không. Đối với mỗi bộ ba, chúng ta sẽ tính toán ba điểm giao nhau theo cặp và kiểm tra xem chúng có khác biệt hay không. Về mặt khái niệm, điều này đơn giản và chính xác, vì bất kỳ tam giác nào cũng phải xuất phát từ ba đường thẳng. 

Tuy nhiên, số lượng bộ ba là$\binom{n}{3}$, đó là về$4.5 \times 10^9$khi$n = 3000$. Ngay cả với khả năng tính toán giao lộ cực nhanh, con số này vẫn vượt xa mức 0,25 giây cho phép. 

Quan sát quan trọng là hình học có cấu trúc chặt chẽ. Chỉ có ba hướng tồn tại, vì vậy bất kỳ hình tam giác nào cũng phải bao gồm chính xác một đường ngang, một đường dọc và một đường chéo. Bất kỳ sự kết hợp nào khác đều không thể: hai đường thẳng song song không thể gặp nhau, do đó có hai đường ngang, hai đường dọc hoặc hai đường chéo không thể tạo thành một ranh giới tam giác khép kín. 

Khi chúng tôi giới hạn bản thân ở một dòng của mỗi loại, các điểm giao nhau sẽ được xác định hoàn toàn. Một đường ngang$y = h$, một đường thẳng đứng$x = v$, và một đường chéo$x + y = d$cắt nhau từng cặp tại:$(v, h)$,$(d - h, h)$, Và$(v, d - v)$. Ba điểm này tạo thành một tam giác trừ khi cả ba đường thẳng gặp nhau tại cùng một điểm, điều này xảy ra chính xác khi$h + v = d$. Điều kiện đó làm cho tất cả các giao điểm thu gọn về một điểm duy nhất, tạo ra một tam giác suy biến. 

Do đó bài toán rút gọn thành việc đếm ba lần$(H, V, D)$như vậy$h + v \ne d$. 

Chúng ta có thể đếm tất cả các bộ ba của một đường ngang, một đường dọc và một đường chéo, sau đó trừ các trường hợp suy biến trong đó$h + v = d$. Phần đầu tiên chỉ đơn giản là$|H| \cdot |V| \cdot |D|$. Phần thứ hai có thể được tính bằng cách lặp qua các cặp$(h, v)$và kiểm tra xem một đường chéo có giá trị$h + v$tồn tại. 

Điều này biến vấn đề thành một nhiệm vụ đếm tần số trên các giá trị lên tới 3000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp ba lần |$O(n^3)$|$O(1)$| Quá chậm | 
| Đếm theo loại + băm |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia các dòng đầu vào thành ba nhóm: ngang, dọc và chéo. Chúng tôi cũng duy trì một mảng tần số hoặc bộ băm cho các hằng số đường chéo để có thể kiểm tra tư cách thành viên trong thời gian không đổi. 

1. Đọc tất cả các dòng và lưu trữ các hằng số ngang trong danh sách$H$, các hằng số dọc trong một danh sách$V$và đánh dấu các hằng số đường chéo trong một mảng hoặc tập hợp boolean$D$. Quá trình xử lý trước này cho phép kiểm tra sự tồn tại nhanh chóng sau này. 
2. Tính tổng số bộ ba được tạo thành bằng cách chọn một dòng từ mỗi họ. Đây là$|H| \cdot |V| \cdot |D|$. Mỗi bộ ba như vậy là một tam giác ứng cử viên trước khi xem xét suy biến. 
3. Lặp lại tất cả các cặp$(h, v)$Ở đâu$h \in H$Và$v \in V$. Tính từng cặp$s = h + v$. Nếu như$s$tồn tại trong tập đường chéo thì bộ ba$(h, v, s)$là suy biến và phải được loại trừ. 
4. Trừ số bộ ba suy biến đó khỏi tổng tích số được tính trước đó. Mỗi hợp lệ$(h, v)$đóng góp chính xác một đường chéo bị cấm, do đó không xảy ra việc đếm quá mức. 
5. Xuất ra số đếm đã sửa cuối cùng. 

Ý tưởng quan trọng là suy biến không phải là một thuộc tính hình học cần tính toán giao điểm đầy đủ, nó hoàn toàn là một điều kiện đẳng thức đại số liên kết ba tham số đường. 

### Tại sao nó hoạt động 

Mỗi tam giác hợp lệ phải sử dụng chính xác một đường thẳng theo mỗi hướng, vì các đường thẳng song song không thể tạo thành các cạnh của tam giác. Cho một phương ngang cố định$y = h$và dọc$x = v$, điểm giao nhau buộc phải là$(v, h)$, và đường chéo phải đi qua điểm này thì tam giác mới sụp đổ. Điều đó xảy ra khi và chỉ khi$h + v = d$. Mỗi cặp như vậy xác định chính xác một hằng số đường chéo, vì vậy việc trừ các trường hợp này sẽ loại bỏ tất cả và chỉ các cấu hình suy biến. Không có sự suy biến nào khác tồn tại bởi vì bất kỳ bộ ba không khớp nào cũng tạo ra ba giao điểm theo cặp riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    H = []
    V = []
    D = [0] * 6005  # since c up to 3000, sums up to 6000

    hasD = set()

    for _ in range(n):
        d, c = input().split()
        c = int(c)
        if d == 'H':
            H.append(c)
        elif d == 'V':
            V.append(c)
        else:
            hasD.add(c)

    total = len(H) * len(V) * len(hasD)

    bad = 0
    for h in H:
        for v in V:
            if (h + v) in hasD:
                bad += 1

    print(total - bad)

if __name__ == "__main__":
    main()
```Việc thực hiện là dịch trực tiếp ý tưởng đếm. Việc tách thành ba danh sách đảm bảo chúng tôi không bao giờ trộn lẫn các hướng không tương thích. Tập đường chéo cho phép kiểm tra tư cách thành viên theo thời gian liên tục khi kiểm tra xem bộ ba có bị suy biến hay không. 

Một điểm tinh tế là mỗi$(h, v)$cặp tương ứng với chính xác một giá trị đường chéo$h+v$, vì vậy việc đếm các trường hợp xấu là tuyến tính theo$|H| \cdot |V|$mà không cần phải tìm kiếm theo đường chéo. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ: 

đầu vào:```
H 0
V 0
D 0
D 1
```Đây$H = \{0\}$,$V = \{0\}$,$D = \{0,1\}$. 

| h | v | h+v | ở D | tệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | vâng | 1 | 

Tổng số gấp ba =$1 \cdot 1 \cdot 2 = 2$. Xấu = 1. Đáp án = 1. 

Điều này chứng tỏ rằng chỉ có đường chéo$x+y=0$gây thoái hóa. 

Bây giờ hãy xem xét: 

đầu vào:```
H 1
H 2
V 3
D 10
```Đây$H = \{1,2\}$,$V = \{3\}$,$D = \{10\}$. 

Tổng số gấp ba =$2 \cdot 1 \cdot 1 = 2$. 

| h | v | h+v | ở D | tệ | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | không | 0 | 
| 2 | 3 | 5 | không | 0 | 

Câu trả lời vẫn là 2, vì không có đường chéo nào khớp với bất kỳ tổng nào. 

Những dấu vết này xác nhận rằng tính thoái hóa được nắm bắt hoàn toàn bởi điều kiện đẳng thức$h+v=d$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| tất cả các cặp đường ngang và dọc đều được kiểm tra một lần | 
| Không gian |$O(n)$| lưu trữ cho các phân vùng dòng và bộ đường chéo | 

Hệ số bậc hai an toàn cho$n \le 3000$bởi vì nó chỉ có hơn hai tập hợp con của đầu vào và các thao tác bên trong vòng lặp là tra cứu hàm băm theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# NOTE: placeholder since full solution is embedded above
# In practice, run main() and capture output

# small sanity checks (conceptual)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác tạo thành ba đơn | 1 | cấu hình hợp lệ tối thiểu | 
| tất cả các dòng song song chỉ có một họ | 0 | không có hình tam giác hợp lệ | 
| nhiều cặp thoái hóa | số lượng đã lọc | tính đúng đắn của việc hủy bỏ đẳng thức | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mọi cặp đường ngang và dọc khớp với một đường chéo. Ví dụ, nếu$H = \{0,1\}$,$V = \{0,1\}$và các đường chéo bao gồm tất cả các tổng$\{0,1,2\}$, thì mỗi cặp tạo ra một cấu hình suy biến. Thuật toán xử lý việc này bằng cách trừ chính xác một cho mỗi cặp, loại bỏ tất cả các hình tam giác. 

Một trường hợp khác là khi không có đường chéo nào tồn tại. Khi đó việc kiểm tra tập hợp luôn thất bại, do đó không có phép trừ nào xảy ra và câu trả lời chỉ đơn giản là$|H| \cdot |V| \cdot 0 = 0$, điều này phản ánh chính xác rằng không có tam giác nào có thể được hình thành mà không có cả ba hướng. 

Trường hợp góc cuối cùng là đầu vào tối thiểu có ít hơn ba hướng hiện diện. Số hạng tích đã trở thành 0, do đó thuật toán tự nhiên trả về 0 mà không cần xử lý đặc biệt.
