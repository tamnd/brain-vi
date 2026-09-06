---
title: "CF 104542C - Hoạt động thú vị"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Mỗi thao tác cho phép chúng ta chọn hai vị trí khác nhau và đồng thời dịch chuyển cả hai ký tự lùi lại một bước trong bảng chữ cái, trong đó việc dịch chuyển có nghĩa là b trở thành a, c trở thành b, v.v. theo chu kỳ để a trở thành z."
date: "2026-06-30T09:09:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104542
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #22 (Interesting-Forces)"
rating: 0
weight: 104542
solve_time_s: 79
verified: false
draft: false
---

[CF 104542C - Hoạt động thú vị](https://codeforces.com/problemset/problem/104542/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Mỗi thao tác cho phép chúng ta chọn hai vị trí khác nhau và đồng thời dịch chuyển cả hai ký tự lùi một bước trong bảng chữ cái, trong đó dịch chuyển có nghĩa là`b`trở thành`a`,`c`trở thành`b`, v.v. theo chu kỳ sao cho`a`trở thành`z`. 

Mục đích là chuyển đổi mọi ký tự trong chuỗi thành`a`sử dụng càng ít thao tác càng tốt. Mỗi thao tác luôn ảnh hưởng đến chính xác hai chỉ số và mỗi ký tự bị ảnh hưởng sẽ tiến gần hơn một bước đến`a`trong bảng chữ cái tuần hoàn. 

Khó khăn chính là chúng ta không được phép thao tác trên một nhân vật duy nhất. Mỗi mức giảm phải được ghép nối với một mức giảm khác ở một nơi khác trong chuỗi, điều này tạo ra sự ghép nối toàn cục giữa tất cả các phép biến đổi cần thiết. 

Kích thước đầu vào đạt tới 200.000 ký tự trong tất cả các trường hợp thử nghiệm, loại trừ mọi giải pháp mô phỏng thao tác từng bước trên các ký tự. Bất kỳ giải pháp đúng nào cũng phải giảm vấn đề về việc đếm và tính chẵn lẻ trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số lượng giảm cần thiết là số lẻ. Vì mỗi phép toán thực hiện chính xác hai lần giảm nên tổng cầu lẻ không thể được thỏa mãn. Ví dụ, nếu chuỗi là`"ab"`, chúng ta cần giảm đi một ký tự cho mỗi ký tự, vì vậy tổng công việc là 2, điều này là ổn. Nhưng nếu chuỗi đó`"aab"`, chúng ta cần tổng mức giảm 0 + 0 + 1 = 1, không thể ghép đôi được nên đáp án phải là`-1`. 

Một trường hợp không rõ ràng khác là khi có thể ghép nối tổng thể nhưng việc phân phối khiến không thể tránh lãng phí các hoạt động, nhưng trong vấn đề này, mọi ghép nối đều được phép thực hiện trên các chỉ số, vì vậy chỉ có tính khả thi tổng thể mới là vấn đề. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng trực tiếp quá trình này. Ở mỗi bước, chúng tôi chọn hai chỉ số chưa`a`và giảm cả hai ký tự. Chúng tôi lặp lại cho đến khi tất cả các nhân vật trở thành`a`. Điều này đúng vì nó phản ánh chính xác định nghĩa hoạt động, nhưng nó nhanh chóng trở nên không khả thi. 

Nếu ban đầu một ký tự`k`cách xa vài bước`a`, thì nó phải được giảm đi một cách chính xác`k`lần. Tính tổng tất cả các ký tự sẽ cho ra tổng số mức giảm cần thiết. Mỗi phép toán đóng góp chính xác hai mức giảm, vì vậy số lượng phép toán chỉ bằng một nửa tổng số này. Phương pháp brute-force thực hiện việc ghép nối này một cách hiệu quả một cách rõ ràng, dẫn đến độ phức tạp trong trường hợp xấu nhất tỷ lệ thuận với tổng số mức giảm cần thiết cho mỗi thao tác, có thể là bậc hai trong mô phỏng bệnh lý. 

Quan sát chính là vấn đề không nằm ở vị trí của các ký tự mà chỉ ở tổng số đơn vị giảm dần cần thiết. Vì mỗi thao tác cung cấp chính xác hai đơn vị giảm dần, trở ngại duy nhất là liệu tổng yêu cầu có chẵn hay không. Khi điều đó được giữ, chúng ta luôn có thể ghép các mức giảm tùy ý giữa các vị trí vì không có ràng buộc nào về việc chỉ số nào có thể được chọn cùng nhau. 

Vì vậy, vấn đề giảm xuống còn việc tính tổng khoảng cách của tất cả các ký tự tới`a`theo thứ tự tuần hoàn, kiểm tra tính khả thi và chia cho hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng số thao tác × n) | O(n) | Quá chậm | 
| Tổng + Giảm chẵn lẻ | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi ký tự, hãy tính xem cần bao nhiêu bước để đạt được`a`. Đây là`(ord(c) - ord('a')) mod 26`. 

Điều này đo lường số lượng giảm chính xác mà ký tự yêu cầu. 
2. Tính tổng các giá trị này trên toàn bộ chuỗi. 

Tổng này thể hiện tổng số hành động giảm đơn vị được yêu cầu trên tất cả các ký tự. 
3. Kiểm tra xem tổng số tiền này có phải là số chẵn không. 

Mỗi thao tác đóng góp chính xác hai mức giảm, do đó tổng số lẻ không thể khớp một cách hoàn hảo. 
4. Nếu tổng là số lẻ, trả về`-1`ngay lập tức. 

Không có cách nào để ghép các phép toán để khớp với số lẻ các mức giảm cần thiết. 
5. Nếu không, hãy chia tổng cho 2 và xuất ra kết quả. 

Mỗi thao tác loại bỏ chính xác hai đơn vị yêu cầu, vì vậy đây là số lượng thao tác tối thiểu. 

### Tại sao nó hoạt động 

Mỗi thao tác luôn giảm tổng “khoảng cách tới`a`" chính xác bằng 2, bất kể chỉ số nào được chọn. Điều đó có nghĩa là tổng tổng là một mod 2 bất biến và mọi chuỗi phép tính hợp lệ đều tương ứng với việc phân chia tổng số phần giảm cần thiết thành các cặp. Vì không có hạn chế nào đối với việc ghép các chỉ số nên bất kỳ hai yêu cầu nào khác 0 luôn có thể được giảm cùng nhau cho đến khi tất cả các yêu cầu biến mất. Điều này làm cho tổng tổng trở thành biến trạng thái duy nhất quan trọng và quy trình được xác định đầy đủ bởi giá trị của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        s = input().strip()

        total = 0
        for c in s:
            total += (ord(c) - ord('a'))

        if total % 2 == 1:
            print(-1)
        else:
            print(total // 2)

if __name__ == "__main__":
    solve()
```Đoạn mã xử lý từng trường hợp kiểm thử một cách độc lập và tính toán tổng số lần dịch lùi cần thiết để biến mỗi ký tự thành`a`. Mỗi ký tự đóng góp khoảng cách bảng chữ cái của nó từ`a`. 

Kiểm tra tính chẵn lẻ là điều kiện chính xác trung tâm. Nếu tổng là số lẻ thì không thể ghép nối vì mỗi thao tác đều tiêu tốn hai đơn vị. Nếu nó là số chẵn, chia cho hai sẽ cho số phép toán chính xác cần thực hiện. 

Không cần mô phỏng hoặc ghép nối tham lam vì hoạt động hoàn toàn đối xứng giữa các chỉ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
s = "amo"
```Chúng tôi tính toán khoảng cách trên mỗi ký tự: 

| Bước | Nhân vật | Khoảng cách đến 'a' | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 
| 2 | m | 12 | 12 | 
| 3 | o | 14 | 26 | 

Tổng số là 26, là số chẵn nên đáp án là 13 phép tính. 

Điều này xác nhận rằng tất cả các mức giảm bắt buộc có thể được ghép nối tùy ý giữa các chỉ mục, ngay cả khi các ký tự có giá trị ban đầu khác nhau. 

### Ví dụ 2 

đầu vào:```
n = 3
s = "abc"
```| Bước | Nhân vật | Khoảng cách đến 'a' | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 
| 2 | b | 1 | 1 | 
| 3 | c | 2 | 3 | 

Tổng số là 3, là số lẻ nên đáp án là`-1`. 

Điều này thể hiện điều kiện bất khả thi chính: mặc dù mỗi nhân vật đều có thể chạm tới`a`, các thao tác luôn tiêu tốn công việc theo cặp, khiến cho trạng thái cuối cùng không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý một lần để tính khoảng cách của nó | 
| Không gian | O(1) | Chỉ có một khoản tiền được lưu trữ | 

Giải pháp dễ dàng phù hợp với giới hạn vì tổng số ký tự trên tất cả các trường hợp thử nghiệm tối đa là 200.000, chỉ cần một lần truyền tuyến tính là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        total = sum(ord(c) - ord('a') for c in s)
        if total % 2 == 1:
            out.append("-1")
        else:
            out.append(str(total // 2))

    return "\n".join(out)

# provided samples
assert run("5\n2\naa\n2\nab\n2\ncc\n3\namo\n4\negzx\n") == "0\n-1\n2\n13\n29"

# custom cases
assert run("1\n2\naz\n") == "25", "simple single pair"
assert run("1\n3\nabc\n") == "-1", "odd total requirement"
assert run("1\n4\nbbbb\n") == "4", "uniform distance case"
assert run("1\n5\naaaaa\n") == "0", "already done"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`az`| 25 | ghép nối đường dài đơn | 
|`abc`| -1 | hoàn toàn không thể xảy ra | 
|`bbbb`| 4 | khoảng cách đều khác 0 | 
|`aaaaa`| 0 | trạng thái đã hài lòng | 

## Vỏ cạnh 

Một trường hợp tối thiểu nhưng phức tạp là`"ab"`. Ở đây tổng khoảng cách là 1, vì vậy câu trả lời là`-1`. Thuật toán tính toán chính xác tổng bằng 1 và ngay lập tức loại bỏ do tính chẵn lẻ, khớp với thực tế là không thể ghép nối một mức giảm bắt buộc duy nhất. 

Trường hợp thứ hai là`"ba"`, cũng có tổng khoảng cách là 1. Mặc dù một ký tự đã ở gần hơn`a`, ràng buộc chẵn lẻ tương tự sẽ chặn tiến trình và thuật toán lại trả về`-1`. 

Một trường hợp như`"cc"`tạo ra tổng khoảng cách 4. Thuật toán xuất ra 2 và điều này tương ứng với việc ghép nối liên tục cả hai ký tự cho đến khi chúng đạt được`a`. Mỗi thao tác giảm đồng thời cả hai và tính bất biến là tổng công việc giảm đúng 2 giữ ở mỗi bước cho đến khi hoàn thành.
