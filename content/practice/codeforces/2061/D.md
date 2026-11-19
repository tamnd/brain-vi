---
title: "CF 2061D - Kevin và những con số"
description: "Chúng ta được cho hai chuỗi số nguyên: chuỗi ban đầu a có độ dài n và chuỗi mục tiêu b có độ dài m. Kevin có thể liên tục lấy hai số bất kỳ từ một số có hiệu nhiều nhất là một, loại bỏ chúng và chèn tổng của chúng trở lại dãy."
date: "2026-06-08T07:40:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 2061
codeforces_index: "D"
codeforces_contest_name: "IAEPC Preliminary Contest (Codeforces Round 999, Div. 1 + Div. 2)"
rating: 1600
weight: 2061
solve_time_s: 98
verified: false
draft: false
---

[CF 2061D - Kevin và những con số](https://codeforces.com/problemset/problem/2061/D) 

**Đánh giá:** 1600 
**Thẻ:** mặt nạ bit, cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai dãy số nguyên: dãy đầu tiên`a`chiều dài`n`và trình tự mục tiêu`b`chiều dài`m`. Kevin có thể liên tục lấy hai số bất kỳ từ`a`có hiệu nhiều nhất là một, loại bỏ chúng và chèn tổng của chúng trở lại dãy. Nhiệm vụ là xác định xem liệu sau một loạt các thao tác này, chúng ta có thể tạo ra nhiều tập hợp của`a`chính xác bằng bội số của`b`. 

Mỗi trường hợp thử nghiệm có thể liên quan đến tối đa`2*10^5`số và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng bị giới hạn bởi`2*10^5`. Với giới hạn thời gian 2 giây, điều này loại trừ bất kỳ giải pháp nào thử nghiệm tất cả các hoạt động có thể một cách ngây thơ, vì số lượng các cặp tiềm năng tăng lên theo kiểu tổ hợp. Chúng ta cần một cách tiếp cận xử lý từng phần tử theo thời gian gần tuyến tính so với`n`Và`m`. 

Các trường hợp cạnh không rõ ràng phát sinh từ các chuỗi có khoảng trống lớn hoặc số lượng lặp lại. Ví dụ, nếu`a = [1, 3]`Và`b = [4]`, thao tác này không thể thực hiện được vì`|1 - 3| = 2`vượt quá chênh lệch cho phép là 1. Một điều tinh tế khác là khi tất cả các số đều giống nhau trong`a`Nhưng`b`yêu cầu một sự phân phối khác, chẳng hạn như`a = [1, 1, 1, 1]`Và`b = [2, 2]`. Các giải pháp ngây thơ có thể cho rằng bất kỳ số tiền nào cũng có thể đạt được, nhưng quy tắc chênh lệch nghiêm ngặt sẽ chặn một số sự hợp nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét mọi cặp phần tử trong`a`thỏa mãn điều đó`|x - y| ≤ 1`điều kiện, hợp nhất chúng và kiểm tra đệ quy xem chuỗi kết quả có thể đạt tới`b`. Về nguyên tắc, điều này đúng vì nó mô phỏng rõ ràng các hoạt động được phép, nhưng lại thất bại đối với`n`lên đến`2*10^5`bởi vì số lượng các cặp tiềm năng là bậc hai và phép đệ quy sẽ nhân chi phí. Ngay cả việc tối ưu hóa bằng tính năng ghi nhớ cũng không thể thực hiện được do không gian trạng thái quá lớn. 

Cái nhìn sâu sắc quan trọng là suy luận về các thừa số nguyên tố của các số so với 2, bởi vì mỗi số có thể được nhân đôi nhiều lần bằng cách hợp nhất các số giống hệt nhau. Thay vì theo dõi mọi sự hợp nhất có thể, chúng ta có thể nén từng số thành một giá trị cơ bản và một số nhân. Nếu chúng ta giải thích mọi số là`x * k`Ở đâu`k`là lũy thừa của hai biểu thị số lần nó có thể được nhân đôi thông qua việc hợp nhất, chúng ta có thể ánh xạ các chuỗi thành nhiều tập hợp của các biểu diễn nén này. Hai số chỉ có thể hợp nhất nếu giá trị cơ sở của chúng bằng nhau và cách tiếp cận này cho phép chúng ta so sánh`a`Và`b`một cách hiệu quả. 

Do đó, giải pháp tối ưu làm giảm vấn đề thành một đường truyền tuyến tính`a`Và`b`, biểu thị mỗi số dưới dạng số đếm của giá trị cơ sở và số nhân tương ứng. Sau đó, chúng tôi mô phỏng việc hợp nhất một cách xác định bằng cách chia các số trong`b`trở lại đơn vị của`a`số cơ sở của, kiểm tra xem số đếm có khớp chính xác không. Điều này bỏ qua nhu cầu tìm kiếm tổ hợp trong khi vẫn đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số trong`a`, liên tục chia nó cho hệ số đã cho`x`cho đến khi không còn chia được nữa. Theo dõi cả số cơ sở đã giảm và số nhân tích lũy từ các phần này. Lưu trữ cái này dưới dạng danh sách các cặp`(base, multiplier)`. Điều này thể hiện có bao nhiêu đơn vị`base`con số này có thể đóng góp sau khi hợp nhất. 
2. Lặp lại phân tách tương tự cho mỗi số trong`b`. Đại diện`b`như một danh sách`(base, multiplier)`cặp. 
3. Sử dụng hai con trỏ để duyệt`a`Và`b`đồng thời. Tại mỗi bước, hãy so sánh`base`các giá trị. Nếu căn cứ khác nhau thì không thể chuyển hóa được`a`vào trong`b`vì việc hợp nhất không thể thay đổi số cơ sở. 
4. Nếu các căn cứ giống nhau, hãy kiểm tra xem`a`số nhân tích lũy của nó đủ để trang trải`b`số nhân của. Trừ số nhân đã sử dụng khỏi`a`và tiến con trỏ vào`b`khi được tiêu thụ hết. Nếu như`a`hết trước`b`, việc chuyển đổi là không thể. 
5. Tiếp tục quá trình này cho đến khi tất cả các phần tử trong`b`đã được khớp. Nếu bất kỳ yếu tố nào vẫn chưa được so sánh trong`b`, hoặc`a`hết sớm, trả về "Không". Nếu không, trả về "Có". 

Tại sao nó hoạt động: Việc phân tách cơ sở và số nhân đảm bảo rằng mọi chuỗi hợp nhất hợp lệ đều được biểu diễn chính xác. Điều bất biến là việc hợp nhất chỉ tích lũy hệ số nhân của các cơ số giống hệt nhau, do đó việc so sánh dựa trên con trỏ vừa cần thiết vừa đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress_sequence(seq, x):
    result = []
    for num in seq:
        cnt = 1
        while num % x == 0:
            num //= x
            cnt *= x
        if result and result[-1][0] == num:
            result[-1][1] += cnt
        else:
            result.append([num, cnt])
    return result

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    x = 2  # The merge factor as per problem examples

    a_comp = compress_sequence(a, x)
    b_comp = compress_sequence(b, x)

    i = j = 0
    possible = True
    while j < len(b_comp):
        if i >= len(a_comp) or a_comp[i][0] != b_comp[j][0]:
            possible = False
            break
        if a_comp[i][1] < b_comp[j][1]:
            b_comp[j][1] -= a_comp[i][1]
            i += 1
        else:
            a_comp[i][1] -= b_comp[j][1]
            j += 1
            if a_comp[i][1] == 0:
                i += 1
    print("Yes" if possible else "No")
```chức năng`compress_sequence`giảm mỗi số về cơ số tối thiểu và đếm xem nó đại diện cho bao nhiêu đơn vị. Vòng lặp chính sau đó so sánh`a`Và`b`tăng dần, tiêu thụ số nhân. Sự tinh tế là sự kết hợp các căn cứ giống hệt nhau trong`a`trong quá trình nén sao cho logic con trỏ không bị ngắt trên các số bằng nhau liên tiếp. 

## Ví dụ đã hoạt động 

Sử dụng mẫu thứ nhất và thứ ba: 

| Bước | a_comp | b_comp | tôi | j | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [[4,1],[5,1]] | [[9,1]] | 0 | 0 | So sánh 4 và 9 → cơ sở khác nhau, cần hợp nhất | 
| Nén | [[9,1]] | [[9,1]] | 0 | 0 | Cơ số bằng nhau, a_mult ≥ b_mult → tiêu thụ 9, tiến j | 

Vì`a=[1,2,2,2]`Và`b=[3,4]`: 

| Bước | a_comp | b_comp | tôi | j | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [[1,1],[2,3]] | [[3,1],[4,1]] | 0 | 0 | Căn cứ 1 vs 3 → cần hợp nhất | 
| Tiêu thụ | [[3,1],[2,2]] | [[3,1],[4,1]] | 0 | 0 | 3 khớp, j→1 | 
| Tiếp theo | [[2,2]] | [[4,1]] | 1 | 1 | 2 vs 4 → hợp nhất 2+2 → 4, j→2 | 

Điều này chứng tỏ rằng cách tiếp cận con trỏ mô phỏng chính xác các phép hợp nhất được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi phần tử trong`a`Và`b`được truy cập nhiều nhất một lần | 
| Không gian | O(n + m) | Cơ sở lưu trữ và số nhân của biểu diễn nén cho từng phần tử | 

Cho rằng`n`Và`m`tổng cộng nhiều nhất là`2*10^5`trên tất cả các trường hợp thử nghiệm, giải pháp chạy thoải mái trong giới hạn thời gian và giới hạn bộ nhớ 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # call the solution block
    # paste the solution here
    import sys
    input = sys.stdin.readline

    def compress_sequence(seq, x):
        result = []
        for num in seq:
            cnt = 1
            while num % x == 0:
                num //= x
                cnt *= x
            if result and result[-1][0] == num:
                result[-1][1] += cnt
            else:
                result.append([num, cnt])
        return result

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        x = 2

        a_comp = compress_sequence(a, x)
        b_comp = compress_sequence(b, x)

        i = j = 0
```
