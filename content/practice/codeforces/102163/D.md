---
title: "CF 102163D - Cúp bóng đá"
description: "Kết quả trận đấu chỉ phụ thuộc vào số bàn thắng cuối cùng. Đối với mọi trường hợp thử nghiệm, X là số bàn thắng được ghi bởi đội của Bashar và Y là số bàn thắng được ghi bởi đội của Hamada."
date: "2026-08-22T18:30:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "D"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 2345
verified: true
draft: false
---

[CF 102163D - Cúp bóng đá](https://codeforces.com/problemset/problem/102163/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kết quả trận đấu chỉ phụ thuộc vào số bàn thắng cuối cùng. Đối với mỗi trường hợp thử nghiệm,`X`là số bàn thắng mà đội của Bashar ghi được và`Y`là con số mà đội Hamada ghi được. Ta cần so sánh hai số nguyên này và in ra tên đội thắng cuộc, hoặc`Iskandar`khi điểm số bằng nhau. 

Câu chuyện về cầu thủ, chấn thương, thay người và thời gian thi đấu không ảnh hưởng đến việc tính toán. Khi đã biết điểm cuối cùng, mọi kết quả có thể xảy ra đều thuộc đúng một trong ba trường hợp:`X > Y`,`X < Y`, hoặc`X == Y`. 

Các giá trị của`X`Và`Y`nhiều nhất là`10^5`, do đó, ngay cả việc quét tuyến tính trên một trường hợp thử nghiệm cũng sẽ không tốn kém. Tuy nhiên, không có lý do gì để quét bất cứ thứ gì vì câu trả lời được xác định bằng một so sánh duy nhất. Với`T`trường hợp thử nghiệm, một`O(T)`giải pháp chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp, dễ dàng thực hiện trong giới hạn 1 giây. Bất kỳ cách tiếp cận nào thực hiện công việc tỷ lệ thuận với điểm số sẽ không cần thiết và một quy trình lồng nhau trên cả hai điểm có thể đạt được khoảng`10^10`hoạt động cho một cặp duy nhất, rõ ràng là không phù hợp. 

Có một vài trường hợp đơn giản có thể bộc lộ việc triển khai bất cẩn. Nếu số điểm bằng nhau thì kết quả phải là`Iskandar`. Ví dụ, đầu vào`3 3`sản xuất`Iskandar`; sử dụng`>=`vì chiến thắng của Bashar sẽ in sai`Bashar`. Nếu Bashar đạt điểm 0 và Hamada đạt điểm cao hơn, chẳng hạn như`0 1`, kết quả là`Hamada`; mã coi số 0 là giá trị chiến thắng đặc biệt sẽ sai. Trường hợp ranh giới ngược`1 0`, phải sản xuất`Bashar`. Cuối cùng,`0 0`là một trận hòa và phải tạo ra`Iskandar`, do đó sự bình đẳng phải được kiểm tra rõ ràng hoặc được thể hiện chính xác bằng cấu trúc so sánh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực có thể mô phỏng sự khác biệt về điểm số bằng cách liên tục thêm một bàn thắng vào số lượng của một đội và cuối cùng so sánh các giá trị kết quả. Ví dụ, để xác định liệu`X`lớn hơn`Y`, người ta có thể liên tục loại bỏ một bàn thắng khỏi cả hai điểm cho đến khi một bàn thắng về 0. Điều này đúng vì mỗi lần loại bỏ theo cặp sẽ giữ nguyên điểm ban đầu nào lớn hơn. Tuy nhiên, trong trường hợp xấu nhất`X = 100000`Và`Y = 99999`, điều này thực hiện gần như`100000`lặp đi lặp lại cho một trường hợp thử nghiệm. Trong một số lượng lớn các trường hợp thử nghiệm, công việc không cần thiết đó có thể trở nên quan trọng, mặc dù câu trả lời thực tế chỉ yêu cầu một so sánh. Một mô phỏng lồng nhau cực đoan hơn có thể thực hiện tới`10^10`hoạt động vượt xa thời hạn. 

Quan sát quan trọng là dữ liệu đầu vào đã chứa số lượng chính xác cần thiết để xác định người chiến thắng. Không có trạng thái ẩn nào để tái tạo và không có chuỗi mục tiêu nào để mô phỏng. So sánh hai điểm số cuối cùng trực tiếp mô tả hoàn toàn kết quả. Brute-force hoạt động vì cuối cùng nó phát hiện ra thứ tự giữa hai số, nhưng thứ tự đó đã có sẵn ngay lập tức thông qua các toán tử so sánh. 

Do đó, giải pháp tối ưu đọc`X`Và`Y`, so sánh chúng một lần và in kết quả tương ứng. Mỗi trường hợp thử nghiệm mất thời gian không đổi, đưa ra`O(T)`tổng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T × max(X, Y)) | O(1) | Chậm một cách không cần thiết | 
| Tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`, vì việc so sánh tương tự phải được thực hiện độc lập cho mỗi trận đấu. 
2. Với mỗi test case, hãy đọc`X`Và`Y`, đại diện cho mục tiêu cuối cùng của đội của Bashar và Hamada. 
3. Nếu`X > Y`, in`Bashar`. Đội của Bashar có nhiều bàn thắng hơn nên là đội chiến thắng. 
4. Ngược lại, nếu`X < Y`, in`Hamada`. Điều kiện trước đó đã loại trừ chiến thắng của Bashar và Hamada thực sự có nhiều bàn thắng hơn. 
5. Ngược lại, hai điểm bằng nhau nên in ra`Iskandar`. Điều này bao gồm các bản vẽ như`0 0`Và`3 3`. 

Tại sao nó hoạt động: trong mỗi trường hợp thử nghiệm,`X`Và`Y`giữ nguyên điểm số cuối cùng. Chính xác là một trong`X > Y`,`X < Y`, hoặc`X == Y`đúng với hai số nguyên. Thuật toán chỉ định từng trường hợp trong số ba trường hợp loại trừ lẫn nhau này cho đầu ra cần thiết của nó, do đó nó không thể tạo ra kết quả không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    x, y = map(int, input().split())

    if x > y:
        print("Bashar")
    elif x < y:
        print("Hamada")
    else:
        print("Iskandar")
```Dòng đầu tiên ghi`T`, điều khiển chính xác số lượng cặp điểm cần được xử lý. sử dụng`sys.stdin.readline`cung cấp đầu vào nhanh chóng trong khi vẫn giữ cho việc thực hiện đơn giản. 

Bên trong vòng lặp,`x`Và`y`được phân tích cú pháp dưới dạng số nguyên. Số nguyên Python không có vấn đề tràn đối với các ràng buộc này và các giá trị thấp hơn nhiều so với bất kỳ giới hạn số nguyên thực tế nào. 

Thứ tự so sánh xử lý ba mối quan hệ có thể có giữa các điểm số. Trường hợp đẳng thức được đặt cuối cùng vì nếu không có bất đẳng thức nào đúng thì khả năng duy nhất còn lại là`x == y`. Không có mối lo ngại riêng lẻ nào vì bài toán chỉ hỏi về thứ tự của các số nguyên cuối cùng. 

Mỗi kết quả được in ngay lập tức nên không cần lưu trữ tất cả các trường hợp kiểm tra hoặc tất cả câu trả lời trong bộ nhớ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy xem xét ba trường hợp thử nghiệm đầu tiên. 

| Trường hợp thử nghiệm |`x`|`y`| So sánh | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 |`1 < 5`|`Hamada`| 
| 2 | 2 | 0 |`2 > 0`|`Bashar`| 
| 3 | 0 | 0 |`0 == 0`|`Iskandar`| 

Trường hợp đầu tiên xác nhận rằng số điểm nhỏ hơn sẽ thua. Trận thứ hai thực hiện ranh giới nơi một đội không có bàn thắng. Điều thứ ba xác nhận rằng sự bình đẳng diễn ra ở nhánh hòa chứ không phải nhánh thắng. 

Đối với các trường hợp mẫu còn lại: 

| Trường hợp thử nghiệm |`x`|`y`| So sánh | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 3 |`3 == 3`|`Iskandar`| 
| 5 | 6 | 2 |`6 > 2`|`Bashar`| 

Trường hợp thứ tư cho thấy một trận hòa được xử lý độc lập với giá trị điểm thực tế. Trường hợp thứ năm xác nhận trường hợp chiến thắng thông thường khi Bashar có số điểm lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một lần so sánh liên tục. | 
| Không gian | O(1) | Chỉ cặp điểm hiện tại và trạng thái vòng lặp được lưu trữ. | 

Giải pháp thực hiện một lượng công việc không đổi nhỏ cho mọi trường hợp thử nghiệm. Ngay cả với giá trị điểm tối đa có thể là`10^5`, độ lớn của điểm số không ảnh hưởng đến thời gian chạy vì thuật toán không bao giờ lặp lại chính các mục tiêu. Việc sử dụng bộ nhớ cũng không đổi và thoải mái dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())

    for _ in range(t):
        x, y = map(int, input().split())

        if x > y:
            print("Bashar")
        elif x < y:
            print("Hamada")
        else:
            print("Iskandar")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""5
1 5
2 0
0 0
3 3
6 2
""") == """Hamada
Bashar
Iskandar
Iskandar
Bashar
""", "sample 1"

assert run("""1
0 0
""") == """Iskandar
""", "minimum scores and draw"

assert run("""3
100000 99999
99999 100000
100000 100000
""") == """Bashar
Hamada
Iskandar
""", "maximum values and adjacent boundary cases"

assert run("""4
1 0
0 1
1 1
2 1
""") == """Bashar
Hamada
Iskandar
Bashar
""", "small scores and equality boundary"

assert run("""2
100000 0
0 100000
""") == """Bashar
Hamada
""", "maximum score against zero")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`Iskandar`| Điểm tối thiểu và sự bình đẳng | 
|`100000 99999`,`99999 100000`,`100000 100000`|`Bashar`,`Hamada`,`Iskandar`| Giá trị tối đa và cả ba kết quả so sánh | 
|`1 0`,`0 1`,`1 1`,`2 1`|`Bashar`,`Hamada`,`Iskandar`,`Bashar`| Giá trị ranh giới nhỏ và so sánh chặt chẽ | 
|`100000 0`,`0 100000`|`Bashar`,`Hamada`| Điểm tối đa so với điểm tối thiểu | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là một trận hòa. Đối với đầu vào`3 3`, đầu tiên thuật toán sẽ kiểm tra`3 > 3`, sai thì kiểm tra`3 < 3`, điều này cũng sai. Nó lọt vào chung kết`else`chi nhánh và bản in`Iskandar`. Lý luận tương tự cũng có tác dụng đối với`0 0`, vì vậy số 0 không yêu cầu một trường hợp đặc biệt riêng. 

Trường hợp thứ hai là Bashar không có mục tiêu trong khi Hamada có mục tiêu. Đối với đầu vào`0 1`,`x > y`là sai và`x < y`là đúng, do đó thuật toán in`Hamada`. Điều này xác nhận rằng số 0 được coi là điểm bình thường. 

Trường hợp ngược lại, đầu vào`1 0`, đến nhánh đầu tiên vì`1 > 0`, sản xuất`Bashar`. Không cần xử lý đặc biệt cho giới hạn dưới. 

Ở ranh giới trên, đầu vào`100000 99999`sản xuất`Bashar`bởi vì sự so sánh đầu tiên là đúng. đầu vào`99999 100000`sản xuất`Hamada`, Và`100000 100000`sản xuất`Iskandar`. Những trường hợp này chứng minh rằng thuật toán vẫn đúng ở giá trị tối đa cho phép và xung quanh ranh giới đẳng thức.
