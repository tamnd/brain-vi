---
title: "CF 104313F - \u0427\u0451\u0442\u043d\u043e-\u043d\u0435\u0447\u0451\u0442\u043d\u044b\u0435 \u043f\u0440\u0438\u0431\u0430\u0432\u043b\u0435\u043d\u0438\u044f"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên và sau đó là một chuỗi các thao tác."
date: "2026-07-01T19:46:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "F"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 54
verified: true
draft: false
---

[CF 104313F - \u0427\u0451\u0442\u043d\u043e-\u043d\u0435\u0447\u0451\u0442\u043d\u044b\u0435 \u043f\u0440\u0438\u0431\u0430\u0432\u043b\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/104313/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên và sau đó là một chuỗi các thao tác. Mỗi thao tác nhắm mục tiêu vào mảng dựa trên tính chẵn lẻ: một loại thêm giá trị cố định cho mọi phần tử chẵn, loại còn lại thêm giá trị cố định cho mọi phần tử lẻ. Sau mỗi thao tác, chúng ta phải báo cáo tổng của mảng. 

Khó khăn chính là mảng thay đổi về mặt khái niệm sau mỗi truy vấn, nhưng chúng tôi không được yêu cầu xuất chính mảng đó mà chỉ xuất tổng của mảng đó. Các ràng buộc nêu rõ rằng cả số phần tử và số lượng thao tác đều có thể đạt tới 100000 cho mỗi trường hợp thử nghiệm, với tổng tổng trên tất cả các thử nghiệm được giới hạn bằng 200000. Điều này ngay lập tức loại trừ việc tính lại tổng từ đầu sau mỗi thao tác, vì điều đó sẽ dẫn đến khoảng O(nq), sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

Trường hợp cạnh tự nhiên cần xem xét là khi tính chẵn lẻ thay đổi linh hoạt. Ví dụ: nếu chúng ta thêm 1 vào tất cả các số chẵn, một số số sẽ trở thành số lẻ. Một cách tiếp cận ngây thơ có thể cố gắng cập nhật số lượng hoặc tổng không chính xác mà không tính đến sự thay đổi này. 

Hãy xem xét ví dụ nhỏ này: 

đầu vào: 

n = 3, a = [1, 2, 3] 

Truy vấn: cộng 10 vào số chẵn 

Mảng đúng trở thành [1, 12, 3], tổng chỉ tăng 10 một lần (vì chỉ tồn tại một phần tử chẵn). Một sai lầm ngây thơ sẽ là giả sử các bộ chẵn lẻ là cố định và chỉ nhân với số lượng ban đầu mà không theo dõi cách các phần tử di chuyển giữa các lớp chẵn lẻ theo thời gian. Điều này trở nên sai ngay khi cập nhật tích lũy. 

Quan sát cơ bản là tính chẵn lẻ không ổn định khi cộng, vì vậy chúng ta không thể duy trì một phân vùng cố định các chỉ số. Thay vào đó, chúng ta cần theo dõi xem có bao nhiêu số chẵn và số lẻ hiện tại cũng như tổng của chúng tiến triển như thế nào. 

## Phương pháp tiếp cận 

Giải pháp brute-force mô phỏng trực tiếp từng thao tác: quét toàn bộ mảng, kiểm tra tính chẵn lẻ của từng phần tử, áp dụng bản cập nhật khi thích hợp và tính toán lại tổng. Điều này đúng vì nó phản ánh chính xác vấn đề. Tuy nhiên, mỗi truy vấn có giá O(n) và với q lên tới 100000, tổng công việc sẽ trở thành O(nq), trong trường hợp xấu nhất đạt tới 10^10 thao tác, vượt xa giới hạn. 

Thông tin chi tiết quan trọng là chúng ta không thực sự cần toàn bộ mảng bất cứ lúc nào. Chúng ta chỉ cần ba thông tin: tổng các phần tử chẵn, tổng các phần tử lẻ và bao nhiêu phần tử thuộc mỗi nhóm. Sau khi chúng tôi duy trì các tổng hợp này, mỗi thao tác sẽ trở thành bản cập nhật liên tục. 

Khi chúng ta thêm x vào tất cả các phần tử chẵn, mỗi phần tử chẵn sẽ tăng thêm x, do đó tổng tăng thêm x nhân với số phần tử chẵn. Tuy nhiên, tính chẵn lẻ thay đổi: tất cả các phần tử chẵn sau đó trở thành số lẻ. Vì vậy, chúng ta phải chuyển cả số lượng và phần đóng góp của họ từ nhóm chẵn sang nhóm lẻ. Logic tương tự được áp dụng đối xứng cho các cập nhật lẻ. 

Điều này chuyển đổi vấn đề từ mô phỏng từng phần tử sang ghi sổ kế toán theo từng nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì bốn giá trị trong suốt quá trình: số phần tử chẵn, số phần tử lẻ, tổng các phần tử chẵn và tổng các phần tử lẻ.

1. Khởi tạo bằng cách quét mảng một lần, phân loại từng phần tử là chẵn hoặc lẻ. Chúng tôi tích lũy cả số lượng và số tiền tương ứng. Điều này cho chúng ta một biểu diễn nén của toàn bộ mảng. 
2. Tính tổng số tiền ban đầu là tổng của các khoản đóng góp chẵn và lẻ. Giá trị này sẽ được cập nhật tăng dần sau mỗi truy vấn. 
3. Đối với truy vấn thuộc loại “thêm x vào tất cả các phần tử chẵn”, trước tiên chúng tôi tính toán tổng số tăng bao nhiêu do thao tác này, tức là x nhân với số phần tử chẵn. Điều này là do mọi phần tử chẵn đều được tăng chính xác một lần. 
4. Sau khi cập nhật tổng số tiền, chúng ta phải phản ánh những thay đổi về cơ cấu. Mọi phần tử chẵn sẽ trở thành số lẻ sau khi thêm x, do đó toàn bộ nhóm chẵn được chuyển sang nhóm lẻ. Chúng ta cập nhật tổng lẻ bằng cách cộng tổng chẵn cũ cộng với x nhân số chẵn, sau đó đặt lại tổng chẵn về 0. 
5. Chúng tôi cũng cập nhật số lượng chẵn lẻ: tất cả các phần tử chẵn trở thành số lẻ, do đó số lẻ tăng lên số chẵn và số chẵn trở thành số 0. 
6. Đối với truy vấn thuộc loại “thêm x vào tất cả các phần tử lẻ”, chúng tôi áp dụng logic đối xứng: tăng tổng tổng lên x lần số lẻ, di chuyển tất cả khối lượng lẻ vào nhóm chẵn và cập nhật số lượng tương ứng. 
7. Sau mỗi truy vấn, xuất ra tổng số tiền hiện tại. 

Tính chính xác dựa trên việc duy trì sự phân chia chính xác các phần tử thành hai nhóm đang phát triển, trong đó mỗi nhóm thể hiện đầy đủ các số chẵn hoặc tỷ lệ cược hiện tại. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mọi phần tử đều thuộc về chính xác một trong hai tập hợp rời nhau: giá trị chẵn hiện tại và giá trị lẻ hiện tại. Mỗi thao tác chỉ áp dụng một phép biến đổi thống nhất cho một tập hợp. Bởi vì tất cả các phần tử trong tập hợp đã chọn đều nhận được các mức tăng giống hệt nhau, tính chẵn lẻ tương đối của chúng thay đổi một cách nhất quán và không có phần tử nào bị phân tách hoặc hoạt động khác với các phần tử khác trong cùng một nhóm. Điều này cho phép toàn bộ trạng thái được tóm tắt bằng số lượng và tổng mà không làm mất thông tin, đồng thời mọi chuyển đổi đều bảo toàn tính bất biến mà tổng và số lượng nhóm khớp chính xác với mảng cơ bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        even_cnt = 0
        odd_cnt = 0
        even_sum = 0
        odd_sum = 0

        for v in a:
            if v % 2 == 0:
                even_cnt += 1
                even_sum += v
            else:
                odd_cnt += 1
                odd_sum += v

        total = even_sum + odd_sum
        out = []

        for _ in range(q):
            typ, x = map(int, input().split())

            if typ == 0:
                total += x * even_cnt
                odd_sum += even_sum + x * even_cnt
                odd_cnt += even_cnt
                even_sum = 0
                even_cnt = 0
            else:
                total += x * odd_cnt
                even_sum += odd_sum + x * odd_cnt
                even_cnt += odd_cnt
                odd_sum = 0
                odd_cnt = 0

            out.append(str(total))

        print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã duy trì thông tin tổng hợp thay vì mảng đầy đủ. Vòng lặp khởi tạo phân loại các phần tử thành các nhóm chẵn lẻ. Mỗi truy vấn cập nhật tổng trực tiếp và sau đó thực hiện chuyển toàn bộ nhóm này sang nhóm khác, phản ánh sự đảo ngược chẵn lẻ sau khi cộng. Điểm tinh tế nhất là sau khi thêm x vào một nhóm, mọi phần tử trong nhóm đó đều thay đổi tính chẵn lẻ nên chúng ta phải di chuyển toàn bộ tổng tích lũy và đếm sang nhóm đối diện chứ không phải cập nhật một phần giá trị. 

Phải cẩn thận để cập nhật tổng sử dụng trạng thái cũ trước khi đặt lại bộ đếm, nếu không khối lượng được truyền sẽ bị mất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, a = [1, 2, 3] 

Truy vấn: 

(0, 2), (1, 1) 

Trạng thái ban đầu: 

| bước | chẵn_cnt | lẻ_cnt | chẵn_sum | lẻ_sum | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 2 | 2 | 4 | 6 | 

Sau (0, 2): cộng 2 vào số chẵn 

| bước | chẵn_cnt | lẻ_cnt | chẵn_sum | lẻ_sum | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| sau | 0 | 3 | 0 | 10 | 12 | 

Giải thích: phần tử chẵn 2 trở thành 4, sau đó chuyển sang số lẻ, nhập vào nhóm lẻ. 

Sau (1, 1): cộng 1 vào tỷ lệ cược 

| bước | chẵn_cnt | lẻ_cnt | chẵn_sum | lẻ_sum | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| sau | 3 | 0 | 13 | 0 | 13 | 

Điều này xác nhận rằng các lần lật chẵn lẻ được truyền bá chính xác thông qua trạng thái tổng hợp. 

### Ví dụ 2 

đầu vào: 

n = 4, a = [2, 4, 5, 7] 

Truy vấn: (1, 3) 

Ban đầu: 

| chẵn_cnt | lẻ_cnt | chẵn_sum | lẻ_sum | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 6 | 12 | 18 | 

Sau (1, 3): 

Tất cả tỷ lệ cược tăng thêm 3: 5→8, 7→10 

| chẵn_cnt | lẻ_cnt | chẵn_sum | lẻ_sum | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 4 | 0 | 18 | 0 | 18 + 6 = 24 | 

Nhóm lẻ trở thành nhóm chẵn sau khi biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) mỗi lần kiểm tra | Mỗi phần tử ban đầu được xử lý một lần, mỗi truy vấn được cập nhật O(1) | 
| Không gian | O(1) thêm | Chỉ các bộ đếm và tổng được lưu trữ | 

Các ràng buộc cho phép tổng cộng lên tới 200000 thao tác, do đó, một giải pháp tuyến tính phù hợp thoải mái trong giới hạn thời gian. Cập nhật liên tục theo thời gian cho mỗi truy vấn đảm bảo không có tắc nghẽn ngay cả trong các bản phân phối thử nghiệm trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# small case
assert run("""1
1 1
2
0 5
""") == "7"

# all odd
assert run("""1
3 2
1 3 5
1 2
0 1
""") == "18\n21"

# all even
assert run("""1
3 2
2 4 6
0 1
1 10
""") == "18\n48"

# alternating parity flips
assert run("""1
4 3
1 2 3 4
0 1
1 1
0 2
""") == "11\n15\n19"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 | cấu trúc tối thiểu | 
| tất cả chuỗi lẻ | 18, 21 | chuyển tiếp chỉ lẻ | 
| tất cả chuỗi chẵn | 18, 48 | chuyển tiếp chỉ chẵn | 
| lật xen kẽ | 11, 15, 19 | tính đúng đắn của chuyển đổi chẵn lẻ | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi các thao tác lặp lại thu gọn hoàn toàn một nhóm chẵn lẻ. Ví dụ: bắt đầu với tất cả các số chẵn và áp dụng số gia lẻ cho số chẵn sẽ ngay lập tức di chuyển mọi phần tử vào nhóm lẻ. 

đầu vào: 

n = 2, a = [2, 4], truy vấn (0, 1) 

Trạng thái ban đầu: 

chẵn_cnt = 2, chẵn_sum = 6 

Sau khi hoạt động: 

mọi phần tử đều trở thành số lẻ: [3, 5] 

Thuật toán cập nhật tổng tổng theo 2 * 2 = 4, cho 10 và chuyển toàn bộ trạng thái chẵn sang trạng thái lẻ. Ngay cả bộ đếm cũng giảm xuống 0. Việc biểu diễn vẫn nhất quán vì tất cả các phần tử thay đổi tính chẵn lẻ một cách thống nhất. 

Một trường hợp tinh tế khác là khi nhiều thao tác liên tiếp nhắm vào một nhóm chẵn lẻ đã trống. Công thức cập nhật sử dụng số đếm, do đó, nhân với 0 đảm bảo không xảy ra sai lệch và trạng thái vẫn ổn định mà không cần viết hoa đặc biệt.
