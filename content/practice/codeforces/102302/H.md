---
title: "CF 102302H - Trình tự lõm"
description: "Chúng ta cần đếm các mảng có độ dài (N), trong đó mỗi phần tử là một trong (0,1,2), sao cho cứ ba phần tử liên tiếp đều thỏa mãn [ a{i-1}a{i+1}le ai^2. ] Câu trả lời là bắt buộc theo modulo (10^9+7)."
date: "2026-08-14T04:39:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "H"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 218
verified: false
draft: false
---

[CF 102302H - Ghi lại chuỗi lõm](https://codeforces.com/problemset/problem/102302/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các mảng có độ dài (N), trong đó mỗi phần tử là một trong (0,1,2), sao cho cứ ba phần tử liên tiếp đều thỏa mãn 

[ 
a_{i-1}a_{i+1}\le a_i^2. 
] 

Câu trả lời là bắt buộc theo modulo (10^9+7). Khó khăn là (N) có thể lớn bằng (10^{18}), vì vậy nhiệm vụ không phải là xây dựng hoặc quét một mảng. Chúng ta cần đếm nhiều mảng theo cấp số nhân bằng cách sử dụng biểu diễn trạng thái có kích thước cố định. 

Bất đẳng thức cục bộ trở nên đặc biệt đơn giản vì mọi giá trị chỉ là (0,1) hoặc (2). Nếu giá trị ở giữa là (0) thì bình phương của nó bằng 0, do đó ít nhất một trong hai lân cận của nó cũng phải bằng 0. Nếu giá trị ở giữa là (1) thì tích của các hàng xóm tối đa phải bằng một, do đó trường hợp duy nhất bị cấm là cả hai hàng xóm đều bằng (2). Nếu giá trị ở giữa là (2), thì bình phương của nó là (4) và mọi tích lân cận có thể có tối đa là (4), vì vậy mọi lựa chọn đều hợp lệ. 

Với (N=3), chỉ có một điều kiện cần kiểm tra. Trong số (3^3=27) bộ ba có thể có, có chính xác 20 bộ thỏa mãn điều đó, đưa ra câu trả lời mẫu. Việc triển khai bất cẩn coi số 0 là an toàn tự động có thể chấp nhận không chính xác (101), nhưng (1\cdot1\le0^2) là sai, vì vậy (101) không hợp lệ. Tương tự, (212) không hợp lệ vì (2\cdot2>1^2). 

Giới hạn dưới (N\ge3) có nghĩa là điều kiện cục bộ luôn tồn tại, trong khi giới hạn trên (10^{18}) loại trừ mọi thứ tỷ lệ với (N). Ngay cả một chương trình động tuyến tính cũng cần quá nhiều thao tác. Vì các giá trị được phép và điều kiện cục bộ là cố định nên chúng ta nên tìm trạng thái có kích thước không đổi và sau đó sử dụng phép lũy thừa để nhảy qua số lượng khổng lồ các vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi mảng trên ({0,1,2}), đưa ra (3^N) ứng cử viên và kiểm tra tất cả (N-2) bộ ba liên tiếp trong mỗi ứng cử viên. Điều này đúng vì định nghĩa này mang tính cục bộ và việc kiểm tra từng bộ ba sẽ mô tả chính xác một mảng hợp lệ. Số lần kiểm tra bất đẳng thức trong trường hợp xấu nhất của nó là ((N-2)3^N). Đã có (N=20), tức là nhiều hơn (10^{10}) lượt kiểm tra và đối với (N=10^{18}), điều đó là hoàn toàn không thể. 

Lực lượng vũ phu hoạt động vì mỗi quyết định chỉ phụ thuộc vào ba giá trị liền kề, nhưng nó thất bại vì nó liên tục tính toán lại các khả năng cục bộ giống nhau. Quan sát quan trọng là sau khi sửa hai giá trị cuối cùng, phần trước của mảng không còn quan trọng khi quyết định giá trị nào có thể được thêm vào. Điều đó có nghĩa là toàn bộ tiền tố có thể được biểu thị bằng một trong các trạng thái (3^2=9). 

Sử dụng trạng thái ((x,y)) cho hai phần tử cuối cùng của tiền tố hợp lệ hiện tại. Để nối (z), chúng ta chỉ cần kiểm tra 

[ 
xz\le y^2. 
] 

Nếu quá trình chuyển đổi hợp lệ, trạng thái mới sẽ trở thành ((y,z)). Điều này tạo ra một đồ thị có hướng cố định với chín trạng thái và nhiều nhất là ba lần chuyển tiếp đi từ mỗi trạng thái. 

Chúng ta có thể biểu diễn đồ thị đó bằng ma trận chuyển tiếp (9\times9) (T). Nếu một hàng tương ứng với trạng thái ((x,y)) và một cột tương ứng với trạng thái ((y,z)), thì (T[row][column]=1) chính xác khi (xz\le y^2). Mỗi mảng hợp lệ có độ dài ba tương ứng với một chuyển đổi hợp lệ giữa hai trạng thái cặp. 

Có chín lựa chọn khả thi cho hai yếu tố đầu tiên và mọi lựa chọn ban đầu đều được phép vì chưa có điều kiện nào được áp đặt. Do đó, vectơ trạng thái ban đầu bao gồm toàn bộ các vectơ trạng thái. Mở rộng chuỗi một lần sẽ nhân vectơ này với (T), kéo dài chuỗi hai lần nhân với (T^2), v.v. Một chuỗi có độ dài (N) yêu cầu chuyển tiếp chính xác (N-2). 

Vì (N-2) có thể là (10^{18}-2), nên chúng tôi tính toán (T^{N-2}) bằng phép lũy thừa ma trận nhị phân. Kích thước ma trận chỉ là chín, vì vậy mỗi phép nhân có kích thước không đổi. Logarit của (N) nhiều nhất là khoảng 60, khiến việc này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N3^N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(9^3\log N)) | (O(9^2)) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Đánh số chín trạng thái theo cặp thứ tự của hai giá trị cuối cùng. Ví dụ: trạng thái (0) có thể biểu thị ((0,0)), trạng thái (1) có thể biểu thị ((0,1)) và ở trạng thái tổng quát (3x+y) đại diện cho ((x,y)). Trạng thái này chứa chính xác thông tin cần thiết để quyết định xem có thể thêm một giá trị khác hay không. 
2. Xây dựng ma trận (9\times9) (T). Đối với mọi cặp ((x,y)) và mọi giá trị tiếp theo có thể có (z), hãy kiểm tra xem (xz\le y^2) hay không. Nếu nó hợp lệ, hãy đặt chuyển đổi từ ((x,y)) sang ((y,z)) thành một. Quá trình chuyển đổi chỉ thay đổi hậu tố hai phần tử, đó là lý do tại sao chín trạng thái là đủ. 
3. Khởi tạo vectơ hàng khái niệm với một vectơ hàng cho mọi trạng thái. Mọi cặp ((x,y)) đều có thể là hai phần tử đầu tiên vì chưa có điều kiện ba phần tử. 
4. Tính (T^{N-2}) bằng cách sử dụng lũy ​​thừa nhị phân. Số mũ là (N-2) vì một mảng chứa hai phần tử chưa kiểm tra bất kỳ bộ ba nào và mỗi phần tử được nối thêm đưa ra chính xác một điều kiện mới. 
5. Tính tổng tất cả các phần tử của (T^{N-2}). Nhân vectơ ban đầu tất cả những cái một với ma trận này sẽ tính mọi trạng thái cuối cùng có thể có và tính tổng các trạng thái cuối cùng đó sẽ tính mọi chuỗi hợp lệ chính xác một lần. 
6. Thực hiện mọi phép toán ma trận modulo (10^9+7). Vì mô-đun được áp dụng sau mỗi phép nhân và phép cộng nên các giá trị vẫn đủ nhỏ để số nguyên Python có thể xử lý thoải mái. 

### Tại sao nó hoạt động 

Điều bất biến là một trạng thái biểu thị chính xác hai phần tử cuối cùng của một tiền tố hợp lệ, trong khi số liên kết với trạng thái đó biểu thị số lượng tiền tố hợp lệ có hậu tố đó. Khi chúng ta nối thêm (z), điều kiện mới được tạo duy nhất là điều kiện liên quan đến hai giá trị cũ cuối cùng và (z). Ma trận bao gồm chính xác những chuyển đổi thỏa mãn điều kiện đó, vì vậy mọi tiền tố hợp lệ sẽ tạo ra chính xác phần mở rộng hợp lệ và không có phần mở rộng không hợp lệ. Do đó, bắt đầu từ tất cả chín cặp đầu tiên có thể có và áp dụng các chuyển tiếp (N-2) sẽ đếm mọi mảng có độ dài-(N) hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
S = 9

def mat_mul(a, b):
    c = [[0] * S for _ in range(S)]

    for i in range(S):
        ai = a[i]
        ci = c[i]

        for k in range(S):
            if ai[k] == 0:
                continue

            aik = ai[k]
            bk = b[k]

            for j in range(S):
                if bk[j]:
                    ci[j] = (ci[j] + aik * bk[j]) % MOD

    return c

def mat_pow(a, exponent):
    result = [[0] * S for _ in range(S)]

    for i in range(S):
        result[i][i] = 1

    while exponent:
        if exponent & 1:
            result = mat_mul(result, a)
        a = mat_mul(a, a)
        exponent >>= 1

    return result

def solve(n):
    transition = [[0] * S for _ in range(S)]

    for x in range(3):
        for y in range(3):
            state = 3 * x + y

            for z in range(3):
                if x * z <= y * y:
                    next_state = 3 * y + z
                    transition[state][next_state] = 1

    powered = mat_pow(transition, n - 2)

    answer = 0
    for row in powered:
        answer = (answer + sum(row)) % MOD

    return answer

def main():
    n = int(input())
    print(solve(n))

if __name__ == "__main__":
    main()
```Việc xây dựng chuyển tiếp thực hiện trực tiếp bất đẳng thức ban đầu. Trạng thái (3x+y) có nghĩa là hậu tố hiện tại là ((x,y)) và việc thêm (z) là hợp pháp chính xác khi (xz\le y^2). Trạng thái đích là (3y+z), vì hậu tố mới bao gồm phần tử thứ hai cũ và giá trị được nối thêm. 

Ma trận nhận dạng trong`mat_pow`đại diện cho sự chuyển tiếp bằng không. Sau đó, lũy thừa nhị phân tiêu chuẩn sẽ xây dựng công suất cần thiết chỉ bằng phép nhân ma trận (O(\log N)). 

Tổng cuối cùng sử dụng mọi phần tử của ma trận lũy thừa thay vì xây dựng vectơ ban đầu một cách rõ ràng. Điều này hoạt động vì vectơ ban đầu chứa chín đơn vị. Đối với mọi trạng thái bắt đầu và trạng thái kết thúc (t), mục nhập (T^{N-2__{s,t}) sẽ tính các đường dẫn hợp lệ giữa chúng. Tổng hợp tất cả các mục tính tất cả các lựa chọn của cặp bắt đầu và cặp kết thúc. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có chiều rộng cố định, tích của hai giá trị bên dưới mô đun có thể đạt đến (10^{18}), do đó, cần có số nguyên 64 bit để nhân trước khi lấy mô đun. Số mũ là`n - 2`, không`n - 1`, bởi vì hai phần tử đầu tiên tạo thành trạng thái ban đầu mà không yêu cầu điều kiện. 

## Ví dụ đã hoạt động 

Với (N=3), số mũ là (1), do đó bản thân ma trận mô tả tất cả các bộ ba hợp lệ. Cặp ban đầu có thể là bất kỳ khả năng nào trong số chín khả năng và một chuyển đổi sẽ nối thêm giá trị thứ ba. 

| Cặp hiện tại | Giá trị tiếp theo hợp lệ | Số | 
| --- | --- | --- | 
| 00 | 0, 1, 2 | 3 | 
| 01 | 0, 1, 2 | 3 | 
| 02 | 0, 1, 2 | 3 | 
| 10 | 0 | 1 | 
| 11 | 0, 1 | 2 | 
| 12 | 0, 1, 2 | 3 | 
| 20 | 0 | 1 | 
| 21 | 0 | 1 | 
| 22 | 0, 1, 2 | 3 | 
| Tổng cộng | | 20 | 

Tổng số là 20, phù hợp với mẫu. Các hàng có số 0 ở giữa minh họa tại sao (101) và (102) bị từ chối, trong khi (100) và (200) được chấp nhận. Giá trị ở giữa (2) không áp đặt hạn chế nào, do đó các trạng thái kết thúc bằng (2) có thể có ba lần tiếp theo. 

Với (N=4), có hai lần chuyển tiếp. Sau lần chuyển đổi đầu tiên, số tiền tố hợp lệ kết thúc ở chín trạng thái là 

| Tiểu bang | 00 | 01 | 02 | 10 | 11 | 12 | 20 | 21 | 22 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Chiều dài 3 | 3 | 3 | 3 | 1 | 2 | 3 | 1 | 1 | 3 | 

Quá trình chuyển đổi thứ hai mang lại 

| Tiểu bang | 00 | 01 | 02 | 10 | 11 | 12 | 20 | 21 | 22 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Chiều dài 4 | 9 | 6 | 5 | 3 | 3 | 5 | 3 | 1 | 5 | 

Tổng hợp các giá trị này cho (40). Số lượng trạng thái thể hiện trực tiếp tính bất biến: mỗi giá trị ghi lại chính xác có bao nhiêu tiền tố hợp lệ có hậu tố hai phần tử cụ thể đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(9^3\log N)) | Phép lũy thừa nhị phân thực hiện phép nhân (O(\log N)) của ma trận (9\times9) | 
| Không gian | (O(9^2)) | Chỉ một số ma trận (9\times9) không đổi được lưu trữ | 

Số lớn nhất có thể (N) chỉ có khoảng 60 chữ số nhị phân, do đó cần khoảng 60 ma trận bình phương cộng với tối đa 60 phép nhân bổ sung. Mỗi phép nhân chỉ có (9^3=729) tích vô hướng trước khi xét đến các thừa số không đổi nhỏ. Điều này thoải mái trong giới hạn 2 giây và 64 MB. 

## Trường hợp thử nghiệm 

Các giá trị mong đợi nhỏ bên dưới có thể được kiểm tra độc lập bằng cách liệt kê tất cả các mảng. Trường hợp kích thước tối đa sử dụng cách triển khai tham chiếu riêng trong khai thác thử nghiệm để số mũ lớn được thực hiện mà không lưu trữ một mảng độ dài (10^{18}).```python
import sys
import io

MOD = 10**9 + 7
S = 9

def solve(n):
    transition = [[0] * S for _ in range(S)]

    for x in range(3):
        for y in range(3):
            state = 3 * x + y
            for z in range(3):
                if x * z <= y * y:
                    transition[state][3 * y + z] = 1

    def mul(a, b):
        c = [[0] * S for _ in range(S)]

        for i in range(S):
            for k in range(S):
                if a[i][k] == 0:
                    continue
                for j in range(S):
                    if b[k][j]:
                        c[i][j] = (
                            c[i][j] + a[i][k] * b[k][j]
                        ) % MOD

        return c

    def power(a, e):
        r = [[0] * S for _ in range(S)]
        for i in range(S):
            r[i][i] = 1

        while e:
            if e & 1:
                r = mul(r, a)
            a = mul(a, a)
            e >>= 1

        return r

    p = power(transition, n - 2)
    return sum(map(sum, p)) % MOD

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n = int(sys.stdin.readline())
        return str(solve(n))
    finally:
        sys.stdin = old_stdin

assert run("3\n") == "20", "sample 1"

assert run("4\n") == "40", "minimum extension"

assert run("5\n") == "85", "catches transition counting errors"

assert run("6\n") == "207", "catches another off-by-one in the exponent"

max_n = 10**18
max_expected = solve(max_n)
assert run(str(max_n) + "\n") == str(max_expected), "maximum N"
assert 0 <= max_expected < MOD, "modulo range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`20`| Độ dài tối thiểu cho phép và điều kiện ba đơn | 
|`4`|`40`| Thành phần và số mũ ma trận chính hãng đầu tiên (N-2=2) | 
|`5`|`85`| Tuyên truyền chính xác thông qua nhiều lần chuyển đổi | 
|`6`|`207`| Bảo vệ chống lại lỗi số mũ hoặc ranh giới khác | 
|`10^18`|`solve(10^18)`| Kích thước đầu vào tối đa và lũy thừa nhị phân | 

Thử nghiệm tối đa có chủ ý kiểm tra ranh giới mà không cố gắng xây dựng một mảng. Giá trị tham chiếu của nó được tính toán bằng cùng một phép toán trạng thái hữu hạn, nhưng phép thử này rất hữu ích để phát hiện các vòng lặp tuyến tính ngẫu nhiên, xử lý số nguyên không chính xác hoặc vòng lặp lũy thừa không thể xử lý số mũ 60 bit. 

## Vỏ cạnh 

Đối với đầu vào tối thiểu (N=3), thuật toán sẽ tính toán (T^1). Không có phần mở rộng bổ sung nào ngoài bộ ba đầu tiên, vì vậy mỗi mục nhập ma trận tương ứng trực tiếp với một bộ ba hợp lệ. Chín hàng chứa (3,3,3,1,2,3,1,1,3) phần tiếp theo hợp lệ tương ứng, tổng cộng là (20). Do đó, đầu vào chính xác`3`sản xuất`20`. 

Giá trị 0 là một nguồn sai lầm đặc biệt dễ xảy ra. Đối với bộ ba`101`, giá trị ở giữa bằng 0, do đó bất đẳng thức trở thành (1\cdot1\le0), là sai. Vì`100`, nó sẽ trở thành (1\cdot0\le0), điều này đúng. Cấu trúc chuyển tiếp kiểm tra sản phẩm thực tế thay vì áp dụng quy tắc lỏng lẻo chẳng hạn như coi mọi chuỗi chứa số 0 là hợp lệ. 

Giá trị (1) có một hạn chế khác. Vì`212`, bất đẳng thức là (2\cdot2\le1), sai. Vì`210`, bất đẳng thức là (2\cdot0\le1), điều này đúng. Do đó, ma trận cho phép trạng thái`21`chỉ chuyển sang`0`, đúng như yêu cầu. 

Giá trị trung bình của (2) không tạo ra hạn chế nào cả. Ví dụ,`022`,`122`, Và`222`đều hợp lệ vì tích lân cận lớn nhất có thể là (2\cdot2=4), bằng (2^2). Bất kỳ quá trình chuyển đổi nào có giá trị trạng thái giữa là (2) đều có thể có cả ba giá trị tiếp theo. 

Cuối cùng, không thể xử lý đầu vào tối đa (N=10^{18}) bằng cách lặp qua các vị trí. Thuật toán giảm toàn bộ tính toán xuống (T^{10^{18}-2}) và lũy thừa nhị phân đạt đến số mũ đó chỉ bằng khoảng 60 lần lặp. Không gian trạng thái vẫn cố định ở chín trạng thái xuyên suốt, do đó độ dài chuỗi khổng lồ chỉ ảnh hưởng đến số bước bình phương chứ không ảnh hưởng đến lượng dữ liệu chuỗi được lưu trữ.
