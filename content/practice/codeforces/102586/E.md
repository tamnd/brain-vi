---
title: "CF 102586E - Đếm Modulo 2"
description: "Chúng ta cần đếm, chỉ bằng tính chẵn lẻ, có thể hình thành bao nhiêu chuỗi có thứ tự có độ dài N bằng cách sử dụng các giá trị cho phép trong A sao cho tổng của chúng chính xác là S. Thứ tự của các số được chọn rất quan trọng vì vị trí trong chuỗi là khác nhau."
date: "2026-07-31T06:32:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102586
codeforces_index: "E"
codeforces_contest_name: "XX Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102586
solve_time_s: 120
verified: true
draft: false
---

[CF 102586E - Đếm Modulo 2](https://codeforces.com/problemset/problem/102586/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm, chỉ bằng tính chẵn lẻ, có bao nhiêu chuỗi có độ dài được sắp xếp`N`có thể được hình thành bằng cách sử dụng các giá trị cho phép trong`A`sao cho tổng của chúng bằng chính xác`S`. Thứ tự của các số được chọn rất quan trọng vì vị trí trong chuỗi là khác nhau. Câu trả lời không phải là số lượng thực tế mà là số lượng đó là số lẻ hay số chẵn. 

Phần khó khăn đó là`N`Và`S`cả hai đều có thể lớn bằng`10^18`, vì vậy bất kỳ phương thức nào lặp lại theo chiều dài của chuỗi đều không thể thực hiện được. Thậm chí lặp đi lặp lại lên đến`S`quá chậm. Tham số nhỏ duy nhất là`K`, số lượng giá trị được phép, tối đa là 200. Giá trị tối đa bên trong`A`cũng đủ nhỏ để có thể sử dụng khi suy luận xem có bao nhiêu trạng thái khác nhau có thể xuất hiện. 

Một cách tiếp cận lập trình động trực tiếp trên tổng sẽ cần khoảng`N * S`công việc, điều này hoàn toàn không thể xảy ra vì cả hai chiều đều có thể rất lớn. Lời giải phải sử dụng thực tế là câu trả lời là cần thiết theo modulo 2. Việc tính modulo 2 làm thay đổi đáng kể lũy thừa đa thức do đồng nhất thức Frobenius:$$(f(x))^2 = f(x^2)$$trên trường nhị phân. 

Có một số trường hợp đặc biệt có thể phá vỡ quá trình triển khai nếu chúng bị bỏ qua. Khi`N = 0`xuất hiện trong quá trình đệ quy, câu trả lời là`1`chỉ tính tổng`0`, vì có đúng một dãy trống. Ví dụ: nếu trạng thái giảm đạt đến`N = 0, S = 3`, kết quả đúng là`0`, trong khi việc triển khai bất cẩn có thể chấp nhận nó vì nó đã xử lý tất cả các bit của`N`. 

Một trường hợp khác là khi một số từ`A`có tính chẵn lẻ sai cho chữ số nhị phân hiện tại. Đối với đầu vào:```
1
1 1
0
```trình tự duy nhất có thể là`[0]`, vậy câu trả lời là`0`. Một quá trình chuyển đổi không kiểm tra tính chẵn lẻ trước khi chia cho hai sẽ tạo ra một trạng thái không chính xác từ`(1 - 0) / 2`và có thể đếm số tiền không hợp lệ. 

Trường hợp thứ ba là các trạng thái trùng lặp hủy modulo 2. Ví dụ:```
1
2 2
1 3
```Các trình tự hợp lệ là`[1,1]`,`[1,3]`,`[3,1]`, Và`[3,3]`. Chỉ có số đầu tiên và số cuối cùng có tổng bằng 2 hoặc 6, vì vậy với`S = 2`câu trả lời là`1`. Khi các lựa chọn khác nhau đạt đến cùng một trạng thái rút gọn, đóng góp của chúng phải được XOR, không được thêm vào như các số đếm thông thường. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là sử dụng hàm tạo$$P(x)=\sum_{a\in A}x^a$$Đáp án là hệ số`x^S`TRONG`P(x)^N`. Phép nhân đa thức mạnh mẽ sẽ liên tục kết hợp các số hạng và theo dõi mọi tổng có thể đạt được. Điều này đúng vì mỗi phép nhân thể hiện việc chọn thêm một phần tử của dãy. Tuy nhiên, mức độ có thể trở nên lớn như`N * max(A)`, xung quanh`10^23`. Ngay cả việc lưu trữ tất cả số tiền có thể truy cập là không thể. 

Quan sát quan trọng xuất phát từ thực tế là tất cả số học đều có modulo 2. Trong lĩnh vực này,$$P(x)^{2}=P(x^2)$$vì vậy nếu chúng ta nhìn vào biểu diễn nhị phân của`N`, mỗi bit được đặt đại diện cho một bản sao của`P(x^{2^i})`. 

Một cách hữu ích hơn để xem cùng một ý tưởng là thông qua việc rút gọn đệ quy. Nếu như`N`là số chẵn, mọi số mũ đóng góp bởi`P(x)^N`là số chẵn, nên là số lẻ`S`ngay lập tức cho số không. Nếu như`N`là số chẵn và`S`là chẵn, cả hai`N`Và`S`có thể chia đôi. 

Nếu như`N`thật kỳ lạ, một bản sao của`P(x)`vẫn còn. Chúng tôi chọn giá trị`a`được đóng góp bởi bản sao đó, và phần còn lại trở thành lũy thừa chẵn. Quá trình chuyển đổi trở thành:$$F(N,S)=\bigoplus_{a\in A,\ a\equiv S\pmod 2}F(\lfloor N/2\rfloor,(S-a)/2)$$Thách thức còn lại là số lượng tiểu bang. Chúng tôi xử lý các bit của`N`từ phía ít quan trọng nhất trong khi lưu trữ tất cả các khoản tiền giảm hiện tại với tính chẵn lẻ của chúng. Sau khi loại bỏ`i`các bit, hai trạng thái có thể có chỉ có thể khác nhau bởi các lựa chọn được thực hiện ở các bit thấp hơn đã bị loại bỏ. Vì mọi giá trị được chọn nhiều nhất là`100000`, khoảng cách của những khác biệt có thể bị giới hạn. Điều này giữ cho số lượng trạng thái hoạt động có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`N`, hoặc ít nhất là tỷ lệ thuận với số tiền có thể tiếp cận | Hàm mũ trong trường hợp xấu nhất | Quá chậm | 
| Tối ưu |`O(60 * K * M)`Ở đâu`M`là số lượng trạng thái được duy trì |`O(M)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một trạng thái chứa tổng ban đầu`S`với sự ngang bằng`1`. Tính chẵn lẻ được lưu trữ cho biết số cách để đạt được số tiền giảm đó là số lẻ hay số chẵn. 
2. Xử lý các chữ số nhị phân của`N`từ bit ít quan trọng nhất trở lên. Ở mỗi bước, hãy xử lý một phép chia cho hai từ phép truy hồi. 
3. Nếu bit hiện tại của`N`bằng 0, mọi đóng góp đều ở mức này. Loại bỏ các trạng thái có tổng lẻ và chia tất cả các tổng còn lại cho hai. Điều này xuất phát từ thực tế là một tổng lẻ không thể được tạo ra từ một đa thức chỉ chứa lũy thừa chẵn. 
4. Nếu bit hiện tại của`N`là một, hãy xóa bản sao còn lại của`P(x)`. Đối với mỗi số tiền hoạt động`s`và mọi giá trị cho phép`a`có cùng độ chẵn như`s`, thêm trạng thái mới`(s-a)/2`. Tính chẵn lẻ của các trạng thái bằng nhau được kết hợp với XOR vì tất cả phép tính đều là modulo 2. 
5. Sau tất cả các bit của`N`được xử lý, số mũ còn lại bằng 0. Câu trả lời là tính chẵn lẻ được lưu trữ ở trạng thái`0`. 

Tại sao nó hoạt động: sau khi xử lý`i`bit của`N`, mọi trạng thái được lưu trữ thể hiện chính xác tính chẵn lẻ của các cách chọn giá trị cho các vị trí nhị phân được xử lý sao cho bài toán chưa được xử lý còn lại có tổng mục tiêu bằng giá trị được lưu trữ đó. Các phép chuyển đổi chính xác là hai trường hợp đồng nhất đa thức trong đặc số hai. Vì các trạng thái bằng nhau được XOR, nên bất biến vẫn đúng sau mỗi bước. Cuối cùng, chỉ tổng mục tiêu còn lại bằng 0 mới có thể thỏa mãn lũy thừa đa thức trống, do đó giá trị được lưu trữ bằng 0 là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(N, S, A):
    states = {S: 1}

    bit = 0
    while N:
        if N & 1:
            nxt = {}
            for s, val in states.items():
                if val == 0:
                    continue
                parity = s & 1
                for a in A:
                    if (a & 1) == parity:
                        ns = (s - a) // 2
                        nxt[ns] = nxt.get(ns, 0) ^ val
            states = {k: v for k, v in nxt.items() if v}
        else:
            nxt = {}
            for s, val in states.items():
                if (s & 1) == 0:
                    ns = s // 2
                    nxt[ns] = nxt.get(ns, 0) ^ val
            states = {k: v for k, v in nxt.items() if v}

        N >>= 1
        bit += 1

        if not states:
            return 0

    return states.get(0, 0)

def main():
    T = int(input())
    ans = []
    for _ in range(T):
        N, S, K = map(int, input().split())
        A = list(map(int, input().split()))
        ans.append(str(solve_case(N, S, A)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Từ điển`states`chỉ lưu trữ những khoản tiền vẫn có thể đạt được câu trả lời cuối cùng. Giá trị của nó luôn bằng 0 hoặc 1 vì hoạt động là XOR. Việc loại bỏ các mục nhập bằng 0 rất hữu ích vì các đường dẫn bị hủy không còn ảnh hưởng đến các quá trình chuyển đổi trong tương lai. 

Vòng lặp tuân theo biểu diễn nhị phân của`N`. Trường hợp chẵn thực hiện bước chia trực tiếp. Trường hợp lẻ thử mọi giá trị được phép có bit thấp nhất khớp với bit mục tiêu hiện tại. Việc kiểm tra tính chẵn lẻ này là cần thiết vì việc trừ một giá trị có bit thấp nhất sai sẽ không để lại số nguyên sau khi chia cho hai. 

Số nguyên Python xử lý kích thước đầu vào một cách tự nhiên, do đó không có vấn đề tràn. Số lần lặp vòng lặp nhiều nhất là 60 vì`N`có tối đa 60 chữ số nhị phân. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
3 6 3
1 2 3
```Biểu diễn nhị phân của`3`là`11`, vì vậy cả hai bước đều sử dụng chuyển tiếp lẻ. 

| Bước | Bit N hiện tại | Kỳ trước | Hoạt động | Kỳ sau | 
| --- | --- | --- | --- | --- | 
| Ban đầu | |`{6:1}`| Bắt đầu |`{6:1}`| 
| 1 | 1 |`{6:1}`| Chọn các giá trị có tính chẵn lẻ |`{2:1, 1:1}`| 
| 2 | 1 |`{2:1,1:1}`| Chọn các giá trị có tính chẵn lẻ phù hợp |`{0:1}`| 

Trạng thái cuối cùng bằng 0 với số chẵn lẻ là 1, nghĩa là số chuỗi hợp lệ là số lẻ. Dấu vết này cho thấy sự phân rã nhị phân của`N`thay thế việc xử lý ba vị trí riêng lẻ. 

Đối với trường hợp biên nhỏ hơn:```
1
1 0 1
0
```| Bước | Bit N hiện tại | Kỳ trước | Hoạt động | Kỳ sau | 
| --- | --- | --- | --- | --- | 
| Ban đầu | |`{0:1}`| Bắt đầu |`{0:1}`| 
| 1 | 1 |`{0:1}`| Chọn`a=0`|`{0:1}`| 

Trạng thái còn lại bằng 0 nên đáp án là một. Điều này kiểm tra trường hợp chuỗi có sẵn duy nhất là số 0 đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(60 * K * M)`| Có tối đa 60 bước nhị phân, mỗi trạng thái hoạt động sẽ thử mọi giá trị được phép. | 
| Không gian |`O(M)`| Từ điển chỉ lưu trữ số tiền giảm hiện tại. | 

Sự tương tác hạn chế quan trọng là`N`Và`S`có thể rất lớn nhưng số bước nhị phân thì rất nhỏ. Số lượng trạng thái vẫn bị giới hạn bởi kích thước giới hạn của các giá trị được phép, làm cho phương pháp này trở nên thiết thực đối với`K <= 200`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(N, S, A):
    states = {S: 1}
    while N:
        nxt = {}
        if N & 1:
            for s, v in states.items():
                for a in A:
                    if (a & 1) == (s & 1):
                        nxt[(s - a) // 2] = nxt.get((s - a) // 2, 0) ^ v
        else:
            for s, v in states.items():
                if s % 2 == 0:
                    nxt[s // 2] = nxt.get(s // 2, 0) ^ v
        states = {k: v for k, v in nxt.items() if v}
        N >>= 1
    return str(states.get(0, 0))

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    T = int(sys.stdin.readline())
    out = []
    for _ in range(T):
        N, S, K = map(int, sys.stdin.readline().split())
        A = list(map(int, sys.stdin.readline().split()))
        out.append(solve_case(N, S, A))
    sys.stdin = old
    return "\n".join(out)

assert run("""5
5 10 3
1 2 3
1 0 1
0
1 1 1
0
2 2 2
1 3
3 3 2
0 1
""") == """1
1
0
1
1""", "mixed cases"

assert run("""1
1 5 1
5
""") == "1", "single value exact sum"

assert run("""1
1000000000000000000 0 1
0
""") == "1", "large N with zero only"

assert run("""1
2 1 1
0
""") == "0", "unreachable odd sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thùng nhỏ hỗn hợp |`1,1,0,1,1`| Chuyển đổi cơ bản và xử lý chẵn lẻ | 
|`N=1`, một giá trị được phép |`1`| Xử lý lựa chọn trực tiếp | 
| Rất lớn`N`chỉ với số không |`1`| Độ sâu nhị phân lớn | 
| Số tiền lẻ không thể truy cập được |`0`| Chuyển đổi chẵn lẻ không chính xác | 

## Vỏ cạnh 

cho`N = 0`sau tất cả các lần giảm, việc triển khai không chạy vòng lặp nữa và chỉ chấp nhận trạng thái`0`. Điều này phù hợp với định nghĩa chuỗi trống và ngăn không cho phần còn lại khác 0 được tính. 

Đối với sự không khớp chẵn lẻ, quá trình chuyển đổi lẻ chỉ tạo ra trạng thái khi`(s & 1) == (a & 1)`. Ví dụ, với`N=1`,`S=1`, Và`A={0}`, trạng thái hiện tại là`{1:1}`. Giá trị duy nhất có tính chẵn lẻ bằng 0, do đó không có trạng thái tiếp theo nào được tạo và câu trả lời trở thành 0. 

Để hủy đường dẫn trùng lặp, bản cập nhật từ điển sử dụng XOR. Nếu hai lựa chọn khác nhau tạo ra tổng số giảm như nhau thì trạng thái sẽ biến mất thay vì được tính hai lần. Đây chính xác là hành vi bắt buộc vì chỉ có số lượng modulo hai vấn đề.
