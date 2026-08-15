---
title: "CF 102367B - Số yêu thích"
description: "Một số nguyên dương có số lẻ các ước số dương chính xác khi nó là một số chính phương. Mọi không phải hình vuông đều có các ước có thể ghép thành d và n/d, trong khi hình vuông có một ước số không ghép đôi, đó là căn bậc hai của nó. Vì vậy, nhiệm vụ có thể được trình bày lại như sau."
date: "2026-08-14T02:58:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102367
codeforces_index: "B"
codeforces_contest_name: "Fall 2019 ICPC-style Waterloo Local Contest"
rating: 0
weight: 102367
solve_time_s: 77
verified: true
draft: false
---

[CF 102367B - Số yêu thích](https://codeforces.com/problemset/problem/102367/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một số nguyên dương có số lẻ các ước số dương chính xác khi nó là một số chính phương. Mọi không phải hình vuông đều có các ước có thể ghép thành d và n/d, trong khi hình vuông có một ước số không ghép đôi, đó là căn bậc hai của nó. 

Vì vậy, nhiệm vụ có thể được trình bày lại như sau. Chúng ta cần mọi số nguyên dương A sao cho cả A và A+K đều là số chính phương. Chúng ta phải in ra có bao nhiêu giá trị như vậy tồn tại, theo sau là các giá trị đó theo thứ tự tăng dần. 

Giả sử 

A=x 2 

và 

A+K=y 2 . 

Vì K>0 nên ta có y>x, và 

K=y 2 −x 2 =(y−x)(y+x). 

Do đó, mọi câu trả lời đều tương ứng với việc phân tích K thành 

d=y−x,e=y+x. 

Từ những định nghĩa này, 

x= 2 e−d ​ . 

Chúng ta cần x ≥1, vì vậy e>d và x phải là số nguyên, do đó d và e phải có cùng tính chẵn lẻ. 

Ràng buộc K<10 9 là chìa khóa để thực hiện. Căn bậc hai của nó nhiều nhất là khoảng 31623, vì vậy chúng ta có thể kiểm tra mọi ước số có thể có cho đến K ​. Đó chỉ là khoảng ba mươi nghìn lần lặp trong trường hợp xấu nhất, đủ nhỏ để có giới hạn một giây. Tuy nhiên, một cách tiếp cận thử mọi A có thể cho đến 10 9 sẽ là quá chậm. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai trực tiếp. Đối với đầu vào K=1, hệ số duy nhất xung quanh ranh giới căn bậc hai là 1⋅1, sẽ cho x=0. Vì A phải dương nên kết quả đúng là không có câu trả lời. Việc triển khai bất cẩn chấp nhận x=0 sẽ bao gồm A=0 không chính xác. 

Đối với đầu vào K=9, hệ số 3⋅3 cũng cho x=0, trong khi 1⋅9 cho x=4. Do đó, đầu ra đúng là 1, theo sau là 16. Việc triển khai chấp nhận các hệ số bằng nhau sẽ báo cáo sai 0 dưới dạng câu trả lời. 

Với đầu vào K=2, không có hai thừa số nào có cùng tính chẵn lẻ. Đầu ra đúng là bằng không. Điều này nắm bắt các triển khai chỉ kiểm tra xem một thừa số có chia K hay không và quên rằng x=(e−d)/2 phải là tích phân. 

Đối với đầu vào K=8, hệ số 2⋅4 cho x=1, do đó A=1. Đầu ra đúng là`1`theo sau là`1`. Đây là một trường hợp biên hữu ích vì K chẵn nhưng vẫn tồn tại các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ liệt kê các giá trị dương có thể có của A, kiểm tra xem A có phải là số bình phương hoàn hảo hay không, kiểm tra xem A+K có phải là số bình phương hoàn hảo hay không và giữ các giá trị hợp lệ. Vì không có giới hạn trên hữu ích của A nhỏ hơn các giá trị do hiệu bình phương gây ra nên việc tìm kiếm trực tiếp có thể yêu cầu kiểm tra theo thứ tự của K ứng viên. Đối với K=10 9, điều đó có nghĩa là khoảng một tỷ ứng viên, thậm chí chưa tính đến chi phí kiểm tra xem mỗi số có phải là số chính phương hay không. Điều này không thể phù hợp với thời hạn. 

Lực lượng vũ phu hoạt động vì nó trực tiếp kiểm tra thuộc tính xác định, nhưng nó bỏ qua phương trình nối hai hình vuông. Sự quan sát 

y 2 −x 2 =(y−x)(y+x)=K 

biến bài toán thành tìm kiếm ước số. Thay vì tìm kiếm trong hàng tỷ giá trị tiềm năng của A, chúng ta tìm kiếm qua các ước của K. Mỗi cặp nhân tố d,e với de=K, d<e và tính chẵn lẻ bằng nhau tạo ra chính xác một giá trị 

A=( 2 e−d ​ ) 2 . 

Ngược lại, mọi A hợp lệ đều tạo ra chính xác một cặp nhân tố như vậy, do đó phép biến đổi này không mất nghiệm. 

Chúng ta chỉ cần kiểm tra d<K ​. Nếu d chia K thì hệ số phù hợp là e=K/d. Việc hạn chế ở d<e sẽ tự động loại trừ x=0, vì d=e có nghĩa là x=0. Sau đó chúng tôi tính toán A tương ứng và sắp xếp kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K) | O(1) không bao gồm đầu ra | Quá chậm | 
| Tối ưu | O( K ​ +RlogR) | O(R) | Đã chấp nhận | 

Ở đây R là số câu trả lời hợp lệ. Vì R nhiều nhất là số ước của K nên thuật ngữ sắp xếp rất nhỏ so với việc quét ước số cho ràng buộc này. 

## Hướng dẫn thuật toán 

1. Đọc K. Chúng ta đang tìm hai bình phương dương x 2 và y 2 có hiệu chính xác là K, với y>x. 
2. Lặp lại mọi ứng viên của ước số nguyên d từ 1 đến ⌊ K ​ ⌋. Nếu d không chia K, thì nó không thể là một trong các giá trị y−x, vì vậy nó có thể bị bỏ qua. 
3. Với mọi ước số d, đặt e=K/d. Cặp (d,e) đại diện cho 

d=y−x,e=y+x. 

Vì d< K ​ nên ta có d<e. 
4. Bác bỏ cặp khi d=e. Trong trường hợp đó, 

x= 2 e−d ​ =0, 

vì vậy A=0, đây không phải là số nguyên dương có số chia hữu hạn. 
5. Loại bỏ cặp khi d và e có số chẵn lẻ khác nhau. Khi đó e−d là số lẻ nên (e−d)/2 không phải là số nguyên. Cặp căn bậc hai thu được sẽ không tồn tại dưới dạng số nguyên. 
6. Với mỗi cặp còn lại, hãy tính 

x= 2 e−d ​ 

và thêm x 2 vào danh sách câu trả lời. Đây chính xác là giá trị tương ứng của A. 
7. Sắp xếp các giá trị đã thu thập và in số lượng của chúng và nếu không trống thì in chính các giá trị đó. Việc sắp xếp là đủ vì việc quét số chia được sắp xếp theo d chứ không phải theo x 2. 

### Tại sao nó hoạt động 

Đối với mỗi câu trả lời được tạo ra, thuật toán có cặp nhân tố d,e thỏa mãn de=K, d<e và tính chẵn lẻ bằng nhau. Việc xác định x=(e−d)/2 cho một số nguyên dương và với y=(e+d)/2 chúng ta thu được 

y 2 −x 2 =(y−x)(y+x)=de=K. 

Do đó A=x 2 và A+K=y 2 đều là số chính phương. 

Ngược lại, lấy bất kỳ A=x 2 và A+K=y 2 hợp lệ nào. Vì K>0, y>x. Đặt d=y−x và e=y+x cho de=K, d<e, và d,e có cùng tính chẵn lẻ vì tổng và hiệu của chúng đều chẵn. Vì một thành viên của mỗi cặp nhân tố có nhiều nhất là K ​, nên thuật toán sẽ kiểm tra d này và xây dựng lại chính xác x và A ban đầu. Do đó, mọi câu trả lời hợp lệ đều được tạo ra và không có câu trả lời không hợp lệ nào được tạo ra. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    k = int(input())
    ans = []
    d = 1    while d * d <= k:        if k % d == 0:            e = k // d
            # d == e would give x = 0, so A = 0.            if d != e and ((d ^ e) & 1) == 0:                x = (e - d) // 2                ans.append(x * x)
        d += 1
    ans.sort()
    print(len(ans))    if ans:        print(*ans)

if __name__ == "__main__":    solve()
```Vòng lặp kết thúc`d`thực hiện tìm kiếm ước số từ thuật toán. điều kiện`d * d <= k`tương đương với d< K ​, nhưng tránh tính căn bậc hai có dấu phẩy động. 

Khi`k % d == 0`,`e = k // d`là hệ số phù hợp. Bài kiểm tra`d != e`loại bỏ việc phân tích nhân tử trong đó cả hai thừa số đều bằng K ​. Việc phân tích nhân tử như vậy sẽ tạo ra x=0, nằm ngoài miền A dương. 

Việc kiểm tra tính chẵn lẻ sử dụng`(d ^ e) & 1`. XOR có bit thấp nhất được đặt chính xác khi hai số nguyên có tính chẵn lẻ khác nhau. Do đó biểu thức bằng 0 chính xác khi`d`Và`e`có cùng độ ngang bằng. Một biểu thức đơn giản hơn như`d % 2 == e % 2`cũng sẽ đúng. 

Khi điều kiện chẵn lẻ được giữ,`(e - d) // 2`là một số nguyên dương căn bậc hai x, và`x * x`là giá trị bắt buộc của A. Số nguyên Python không bị tràn, do đó không cần xử lý độ rộng số nguyên đặc biệt. 

Cuối cùng, các câu trả lời được sắp xếp trước khi in. Dòng đầu ra đầu tiên luôn được in ngay cả khi không có câu trả lời. Dòng thứ hai chỉ được in khi danh sách câu trả lời không trống, phù hợp với định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Không có cặp đầu vào/đầu ra mẫu có thể sử dụng nào xuất hiện trong văn bản câu lệnh được cung cấp, vì vậy các dấu vết sau đây sử dụng hai đầu vào cụ thể nhỏ. 

Với K=8, cặp số chia là 1⋅8 và 2⋅4. Chỉ có cặp thứ hai có tính chẵn lẻ bằng nhau và các thừa số khác biệt. 

|`d`|`e = K // d`|`d == e`| Tính chẵn lẻ giống nhau |`x = (e-d)//2`|`A = x*x`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | Không | Không | | | 
| 2 | 4 | Không | Có | 1 | 1 | 

Thuật toán tạo ra một câu trả lời,`A=1`. Thật vậy, 1 và 1+8=9 đều là số chính phương. Dấu vết cũng chứng minh tại sao việc kiểm tra tính chẵn lẻ là cần thiết. 

Với K=9, các cặp ước số lên tới 9 ​ là 1⋅9 và 3⋅3. 

|`d`|`e = K // d`|`d == e`| Tính chẵn lẻ giống nhau |`x = (e-d)//2`|`A = x*x`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | Không | Có | 4 | 16 | 
| 3 | 3 | Có | Có | | | 

Cặp đầu tiên cho x=4, do đó A=16 và 16+9=25. Cặp thứ hai bị từ chối vì nó sẽ cho x=0. Dấu vết này chứng minh tại sao các yếu tố bằng nhau không được chấp nhận mặc dù tính chẵn lẻ của chúng là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( K ​ +RlogR) | Chúng tôi kiểm tra tất cả các ứng cử viên ước số cho đến K ​, sau đó sắp xếp các câu trả lời R. | 
| Không gian | O(R) | Danh sách câu trả lời lưu trữ mọi giá trị hợp lệ của A. | 

Với K<10 9, vòng chia thực hiện tối đa khoảng 31623 lần lặp. Điều đó có thể dễ dàng quản lý trong giới hạn 1 giây đã nêu và số lượng ước số đủ nhỏ để việc lưu trữ và sắp xếp các câu trả lời cũng không tốn kém. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với 256 MB. 

## Trường hợp thử nghiệm 

Văn bản câu lệnh gốc được cung cấp ở đây không chứa cặp đầu vào/đầu ra mẫu hợp lệ, vì vậy bộ thử nghiệm bên dưới sử dụng các trường hợp dẫn xuất độc lập. Trình trợ giúp thực hiện phép biến đổi được yêu cầu tương tự để tạo ra kết quả mong đợi cho bài kiểm tra giá trị tối đa, thay vì nhúng một danh sách câu trả lời dài được viết thủ công.```python
Python# helper: run the solution on an input stringimport sysimport io

def solve():    input = sys.stdin.readline    k = int(input())
    ans = []
    d = 1    while d * d <= k:        if k % d == 0:            e = k // d            if d != e and ((d ^ e) & 1) == 0:                x = (e - d) // 2                ans.append(x * x)        d += 1
    ans.sort()
    print(len(ans))    if ans:        print(*ans)

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| K nhỏ nhất có thể và bác bỏ A=0. | 
|`9`|`1`theo sau là`16`| Cặp yếu tố bằng nhau phải bị từ chối. | 
|`8`|`1`theo sau là`1`| Hợp lệ ngay cả K và xử lý chẵn lẻ. | 
|`2`|`0`| Ngay cả K cũng không chia hết cho 4. | 
|`1000000000`| Được tạo độc lập | Hiệu suất ranh giới đầu vào và vòng chia tối đa. | 

## Vỏ cạnh 

Đối với K=1, vòng lặp kiểm tra d=1, thu được e=1 và loại bỏ cặp vì các thừa số bằng nhau. Về mặt đại số, điều này sẽ cho x=0 và A=0, vì vậy kết quả đầu ra đúng là`0`. Việc xử lý đặc biệt không phải là một hạn chế tùy ý: số 0 nằm ngoài miền số nguyên dương và không có số chia hữu hạn. 

Với K=9, trước tiên thuật toán tìm 1⋅9. Cả hai thừa số đều lẻ, nên x=(9−1)/2=4, tạo ra A=16. Sau đó nó tìm thấy 3⋅3, nhưng từ chối nó vì các thừa số bằng nhau. Đầu ra cuối cùng là`1`Và`16`. Nếu không kiểm tra tính bằng nhau, thuật toán sẽ bao gồm số 0 không chính xác. 

Với K=2, cặp thừa số duy nhất là 1⋅2. Các yếu tố có tính chẵn lẻ khác nhau nên`(e-d)/2`không phải là số nguyên. Danh sách câu trả lời vẫn trống và chương trình sẽ in`0`. Tổng quát hơn, mọi K≡2(mod4) đều có hành vi này, bởi vì tích của hai số nguyên có cùng số chẵn lẻ là số lẻ hoặc chia hết cho 4. 

Với K=8, cặp 2⋅4 có các thừa số chẵn lẻ bằng nhau và khác biệt. Nó cho x=(4−2)/2=1, do đó A=1 và A+K=9. Chương trình in`1`theo sau là`1`, xác nhận rằng các giá trị chẵn của K có thể có câu trả lời hợp lệ khi chúng chia hết cho 4. 

Đối với đầu vào tối đa K=10 9, vòng lặp vẫn dừng chỉ sau 31623 ứng cử viên ước số vì nó phụ thuộc vào K ​, không phụ thuộc vào chính K. Đây là lý do chính khiến giải pháp vẫn nhanh ở ranh giới ràng buộc trên.
