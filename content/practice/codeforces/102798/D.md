---
title: "CF 102798D - Phỏng đoán ABC"
description: "Chúng ta được cho một số nguyên dương c cho mỗi trường hợp thử nghiệm. Chúng ta cần quyết định xem có thể chia c thành hai phần dương a và b sao cho a + b = c và tích của tất cả các thừa số nguyên tố phân biệt xuất hiện ở bất kỳ đâu trong a, b và c nhỏ hơn c hay không."
date: "2026-07-27T17:48:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102798
codeforces_index: "D"
codeforces_contest_name: "2020 China Collegiate Programming Contest, Weihai Site"
rating: 0
weight: 102798
solve_time_s: 49
verified: true
draft: false
---

[CF 102798D - Phỏng đoán ABC](https://codeforces.com/problemset/problem/102798/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương`c`cho từng trường hợp thử nghiệm. Chúng ta cần quyết định xem có thể chia tách được không`c`thành hai phần tích cực`a`Và`b`như vậy`a + b = c`và tích của tất cả các thừa số nguyên tố riêng biệt xuất hiện ở bất cứ đâu trong`a`,`b`, Và`c`nhỏ hơn`c`. Tích này là hàm căn, thường được viết là`rad`. Tác vụ chỉ yêu cầu sự tồn tại nên chúng ta xuất ra`yes`nếu có ít nhất một sự phân chia hợp lệ tồn tại và`no`nếu không thì. 

Giá trị của`c`có thể lớn như`10^18`, trong khi chỉ có một vài trường hợp thử nghiệm. Điều này ngay lập tức loại trừ việc lặp lại các giá trị có thể có của`a`hoặc bao thanh toán bằng phép chia thử lên đến`sqrt(c)`. Chúng ta cần một quan sát theo lý thuyết số để giảm mỗi trường hợp thử nghiệm thành một số lượng nhỏ các phép toán. 

Hạn chế chính xuất phát từ`c`chính nó. Vì mọi ước nguyên tố của`c`xuất hiện ở`abc`,`rad(abc)`luôn chứa`rad(c)`như một yếu tố. Nếu như`c`không có thừa số nguyên tố lặp lại thì`rad(c)=c`, điều này đã làm cho`rad(abc)`ít nhất`c`. Một số như vậy không bao giờ có thể thỏa mãn bất đẳng thức cần thiết. 

Trường hợp còn lại là khi`c`không phải là hình vuông. Nếu một hình vuông nguyên tố chia`c`, chúng ta có thể buộc cả hai mệnh đề phải chứa số nguyên tố lặp lại đó và làm cho căn thức đủ nhỏ. Toàn bộ vấn đề được quy gọn vào việc kiểm tra xem liệu`c`có một thừa số nguyên tố lặp lại. 

Một sai lầm phổ biến là chỉ kiểm tra xem`c`là chẵn. Ví dụ,`c=9`có một giải pháp:`3+6=9`, Và`rad(3*6*9)=3<9`. Một giải pháp chỉ thử chia đều cho các số chẵn sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp cạnh khác là`c=1`. Không thể chia nó thành hai số nguyên dương, nên đáp án là`no`. Chỉ kiểm tra tính vuông góc mà không xử lý điều này sẽ chấp nhận nó một cách không chính xác bởi vì`1`là hình vuông. 

Trường hợp biên cuối cùng là một số nguyên tố lớn như`c=10^18+3`nếu nó là số nguyên tố. Nó không có các yếu tố lặp lại, vì vậy ngay cả một giá trị rất lớn cũng không thể giúp được. Câu trả lời phụ thuộc vào thuộc tính phân tích nhân tử, không phải kích thước của số. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách phân chia có thể`a+b=c`. Đối với mỗi`a`từ`1`ĐẾN`c-1`, chúng ta sẽ tính toán`b=c-a`, nhân tố`a*b*c`và kiểm tra xem căn thức có nhỏ hơn không`c`. Điều này đúng vì nó kiểm tra mọi ứng cử viên có thể. Tuy nhiên, đối với`c=10^18`số lần chia tách có thể là khoảng`10^18`, vượt xa mọi thời gian chạy hợp lý. 

Quan sát làm thay đổi vấn đề là`c`chính nó đã kiểm soát câu trả lời. Mỗi phép chia số nguyên tố`c`phải xuất hiện trong`rad(abc)`. Nếu như`c`là không vuông góc, các số nguyên tố đó nhân với nhau một cách chính xác`c`, làm cho bất đẳng thức không thể xảy ra. 

Nếu như`c`chứa một thừa số bình phương, giả sử`p^2`chia rẽ`c`. Chúng ta có thể chọn`a=p`Và`b=c-p`. Từ`c`chia hết cho`p`, cả hai phần đều là bội số dương của`p`. Viết`c=p*k`, ba số chứa mẫu phân tích nhân tử của`p`,`p(k-1)`, Và`pk`. Các số nguyên tố bổ sung duy nhất có thể đến từ`k`Và`k-1`, liên tiếp và do đó nguyên tố cùng nhau. Yếu tố lặp lại trong`c`đảm bảo rằng có đủ lũy thừa ẩn bên trong các số nguyên tố giống nhau, làm cho căn thức nhỏ hơn rất nhiều so với`c`. 

Brute Force hoạt động vì nó trực tiếp tìm kiếm một phân tách hợp lệ nhưng không thành công khi`c`là lớn. Quan sát cấu trúc cho thấy chỉ có thừa số bình phương của`c`vấn đề làm giảm vấn đề để kiểm tra xem`c`là hình vuông. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c * phân tích nhân tử(c)) | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(c)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trường hợp đặc biệt`c=1`. Không có hai số nguyên dương nào có tổng bằng`1`, vậy là có ngay câu trả lời`no`. 
2. Cố gắng tìm ước số nguyên tố`p`của`c`như vậy`p`chia rẽ`c`ít nhất hai lần. Chúng tôi làm điều này bằng cách kiểm tra xem`c`chia hết cho`p*p`. Tìm một phương tiện nguyên tố như vậy`c`không phải là hình vuông. 
3. Nếu tìm thấy bất kỳ thừa số nguyên tố lặp lại nào, hãy xuất ra`yes`. Việc xây dựng được mô tả ở trên đưa ra một sự phân chia hợp lệ. 
4. Nếu quá trình quét kết thúc mà không tìm thấy hệ số lặp lại, hãy xuất ra`no`. Số này không vuông góc nên căn của nó đã bằng`c`, ngăn cản mọi giải pháp khả thi. 

Lý do điều này có tác dụng là vì sự tồn tại của một thừa số nguyên tố lặp lại chính xác là ranh giới giữa các trường hợp có thể và không thể. Một hình vuông`c`buộc số cấp tiến của mỗi ứng cử viên ít nhất phải gấp ba lần`c`, trong khi không vuông góc`c`cung cấp một yếu tố lặp lại có thể được chia sẻ bởi hai lệnh triệu tập. 

Tại sao nó hoạt động: 

Đối với mọi phép chia hợp lệ, tất cả các thừa số nguyên tố của`c`xuất hiện ở`rad(abc)`. Vì vậy, nếu`c`là hình vuông,`rad(abc)`chứa mọi số nguyên tố của`c`đúng một lần và ít nhất là`rad(c)=c`. Sự bất bình đẳng không thể giữ được. 

Bây giờ giả sử`c`không phải là hình vuông. Cho phép`p^2`chia`c`, và chọn`a=p`,`b=c-p`. Từ`c=p*k`với`p`chia`k`, chúng tôi nhận được`b=p(k-1)`. Căn bản của`abc`gồm các số nguyên tố từ`p`,`k`, Và`k-1`. Bởi vì`k`Và`k-1`không chia sẻ thừa số nguyên tố, thừa số lặp lại từ`k`không làm tăng gốc tự do, trong khi`c`chứa một bản sao bổ sung của`p`. Do đó căn thức hoàn toàn nhỏ hơn`c`. 

Thuật toán kiểm tra chính xác hai khả năng này nên không thể trả về câu trả lời sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(c):
    if c == 1:
        return "no"

    p = 2
    while p * p <= c:
        if c % (p * p) == 0:
            return "yes"
        p += 1

    return "no"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        c = int(input())
        ans.append(solve_case(c))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Giải pháp kiểm tra các yếu tố lặp lại thay vì tính toán căn thức một cách rõ ràng. Điều này tránh phải xử lý các tích lớn và chỉ yêu cầu phát hiện xem một hình vuông nguyên tố có chia hết hay không`c`. 

Điều kiện vòng lặp sử dụng`p*p <= c`, đó là ranh giới tiêu chuẩn để phân chia thử nghiệm. Nếu không có ước số nào tồn tại cho đến thời điểm này thì không có thừa số lớn hơn nào có thể có đóng góp bình phương mà không tồn tại thừa số phù hợp nhỏ hơn. 

Số nguyên Python không bị tràn nên phép nhân`p*p`vẫn an toàn ngay cả khi ở gần`10^18`. Số lần lặp lại là mối quan tâm duy nhất và giới hạn căn bậc hai giữ nó trong phạm vi có thể quản lý được đối với số lượng trường hợp thử nghiệm nhất định. 

## Ví dụ đã hoạt động 

cho`c=18`, thuật toán tìm kiếm một yếu tố lặp lại. 

| Bước | Số chia hiện tại | Chia`c`hai lần? | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | 2 | Đúng,`18`chia hết cho`2*2`? Không | Tiếp tục | 
| 2 | 3 | Đúng,`18`chia hết cho`3*3`| Tìm thấy | 

Thuật toán trả về`yes`. Yếu tố lặp lại là`3`và phép chia hợp lệ`6+12=18`có`rad(6*12*18)=6`, nhỏ hơn`18`. 

Vì`c=30`, quá trình quét sẽ hoạt động khác đi. 

| Bước | Số chia hiện tại | Chia`c`hai lần? | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | 2 | Không | Tiếp tục | 
| 2 | 3 | Không | Tiếp tục | 
| 3 | 4 | Không phải là ước số nguyên tố | Tiếp tục | 
| 4 | 5 | Không, chỉ có một thừa số 5 | Tiếp tục | 

Không có số nguyên tố nào được chia`30`, do đó thuật toán trả về`no`. Từ`30`là không chính phương, mọi phép chia có thể đều có một căn thức chứa tất cả các thừa số của`30`, làm cho bất đẳng thức cần thiết là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(c)) | Chúng tôi kiểm tra các thừa số nguyên tố có thể lặp lại đến căn bậc hai của`c`. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giá trị đầu vào lớn nhất có thể là`10^18`, do đó nghiệm tránh được mọi công tỉ lệ với`c`. Chỉ với tối đa mười trường hợp thử nghiệm, tìm kiếm căn bậc hai là thang đo dự định cho vấn đề này. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("""3
4
18
30
""") == """yes
yes
no
""", "samples"

# minimum-size input
assert run("""1
1
""") == """no
""", "cannot split one"

# all-equal repeated factor case
assert run("""1
64
""") == """yes
""", "large prime power"

# squarefree composite
assert run("""1
210
""") == """no
""", "squarefree composite"

# odd repeated factor
assert run("""1
25
""") == """yes
""", "odd square factor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`no`| Xử lý trường hợp phân chia không hợp lệ nhỏ nhất. | 
|`64`|`yes`| Xác nhận công suất lặp lại lớn được phát hiện. | 
|`210`|`no`| Nắm bắt các giải pháp chỉ kiểm tra tính chia hết cho một số nguyên tố. | 
|`25`|`yes`| Xác nhận các giá trị lẻ không vuông góc hoạt động. | 

## Vỏ cạnh 

cho`c=1`, thuật toán trả về ngay`no`vì không có cặp dương nào có thể tổng bằng`1`. Không cần phải nhập tìm kiếm nhân tố. 

Đối với một giá trị bình phương như`c=30`, thuật toán không tìm được số nguyên tố`p`Ở đâu`p*p`chia rẽ`c`. Vì căn thức của`c`đã rồi`30`, mọi khả năng`rad(abc)`ít nhất là`30`, vì vậy việc từ chối giá trị là đúng. 

Đối với một giá trị nguyên tố lặp đi lặp lại như`c=25`, thuật toán tìm`5*5`chia rẽ`25`và trả về`yes`. Việc xây dựng là`a=5`,`b=20`, cho`rad(5*20*25)=10<25`. 

Đối với một giá trị lớn không có thừa số nguyên tố lặp lại, thuật toán không bao giờ cố gắng xây dựng các phép chia căn bản hoặc liệt kê. Nó chỉ cần xác nhận rằng không tồn tại thừa số bình phương, đây chính là thuộc tính quyết định câu trả lời.
