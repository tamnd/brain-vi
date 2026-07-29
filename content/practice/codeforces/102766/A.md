---
title: "CF 102766A - Singhal và Hoán đổi"
description: "Bài toán đưa ra hai chuỗi chữ thường, S và T. Một thao tác chọn một vị trí từ S và một vị trí từ T và trao đổi các ký tự tại các vị trí đó. Hoạt động có thể được lặp lại bất kỳ số lần."
date: "2026-07-28T23:46:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "A"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 64
verified: true
draft: false
---

[CF 102766A - Singhal và Hoán đổi](https://codeforces.com/problemset/problem/102766/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra hai chuỗi chữ thường,`S`Và`T`. Một hoạt động chọn một vị trí từ`S`và một vị trí từ`T`và trao đổi các ký tự tại các vị trí đó. Hoạt động có thể được lặp lại bất kỳ số lần. Mục đích là tạo ra phiên bản cuối cùng của`S`càng nhỏ càng tốt theo thứ tự từ điển. Tuyên bố ban đầu và các ví dụ là từ Codeforces Gym 102766A. 

Phần quan trọng của thao tác là các ký tự không bao giờ biến mất hoặc xuất hiện. Chúng chỉ di chuyển giữa hai dây. Kể từ chuỗi cuối cùng`S`có cùng độ dài với bản gốc`S`, câu hỏi duy nhất là những ký tự nào trong nhóm kết hợp của`S`Và`T`nên ở lại bên trong`S`. 

Các ràng buộc nhỏ, với mỗi chuỗi có độ dài tối đa là 100 và nhiều nhất là 100 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả giải pháp sắp xếp các ký tự kết hợp cũng đủ nhanh. Cách tiếp cận bậc hai hoặc thậm chí tệ hơn một chút vẫn có khả năng vượt qua, nhưng việc hiểu cấu trúc cho phép chúng ta giải quyết nó trực tiếp theo thời gian tuyến tính sau khi đếm các ký tự. 

Những lỗi phổ biến xuất phát từ việc xử lý các chuỗi một cách riêng biệt. Ví dụ: nếu chúng ta chỉ sắp xếp`S`, chúng tôi bỏ lỡ các ký tự hữu ích có sẵn trong`T`. 

Hãy xem xét đầu vào này:```
1
ba
c
```Đầu ra đúng là:```
ab
```Một giải pháp bất cẩn chỉ sắp xếp lại các ký tự đã có bên trong`S`sẽ giữ`ba`, nhưng hoán đổi`c`với`b`cho`ca`, và sau đó trao đổi là không hữu ích. Mức tối thiểu thực tế đạt được bằng cách lấy hai ký tự nhỏ nhất từ ​​nhóm kết hợp`{a,b,c}`, đó là`a`Và`b`. 

Một trường hợp cạnh khác là khi hai chuỗi chứa nhiều ký tự bằng nhau.```
1
aaa
aaa
```Đầu ra đúng là:```
aaa
```Một giải pháp giả định mỗi lần hoán đổi thay đổi chuỗi có thể thực hiện công việc không cần thiết hoặc cố gắng thay thế các ký tự bằng nhau không chính xác. 

Trường hợp cạnh cuối cùng là khi tất cả các ký tự nhỏ nhất đều nằm trong`T`.```
1
zzz
abc
```Đầu ra đúng là:```
abc
```Một giải pháp chỉ xem xét việc cải thiện vị thế bằng cách sử dụng hoán đổi trực tiếp từng cái một có thể dừng lại quá sớm. Hoạt động có thể được lặp đi lặp lại, vì vậy mọi vị trí trong`S`cuối cùng có thể nhận được các nhân vật tốt nhất hiện có trên toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng các giao dịch hoán đổi và khám phá các chuỗi khác nhau có thể đạt được. Đối với mỗi lần hoán đổi có thể xảy ra, nó có thể tạo ra một trạng thái khác và tiếp tục cho đến khi không có trạng thái mới nào xuất hiện. Điều này đúng vì mọi chuỗi hoạt động hợp pháp đều được thể hiện ở đâu đó trong tìm kiếm. Vấn đề là số lượng trạng thái có thể tăng lên cực kỳ nhanh chóng. Nếu chiều dài kết hợp là`m`, số cách chọn`|S|`nhân vật thuộc về`S`đã rồi`C(m, |S|)`, trước khi xem xét các đơn đặt hàng khác nhau. Ngay cả đối với độ dài vừa phải, điều này trở nên không thể. 

Brute Force hoạt động vì nó khám phá chính xác các trạng thái có thể tiếp cận nhưng không thành công vì có quá nhiều trạng thái có thể tiếp cận. Quan sát mở ra vấn đề là trình tự hoán đổi chính xác không thành vấn đề. Điều duy nhất quan trọng là có nhiều tập ký tự kết thúc bằng`S`. 

Mỗi lần hoán đổi sẽ chuyển một ký tự vào và một ký tự ra. Vì chúng ta có thể chọn bất kỳ vị trí nào trong`S`và bất kỳ vị trí nào trong`T`, chúng ta có thể thay thế nhiều lần các ký tự không mong muốn trong`S`với các ký tự nhỏ hơn từ`T`. Điều này có nghĩa là trận chung kết`S`có thể chứa bất kỳ`|S|`các ký tự từ multiset kết hợp. 

Để thu nhỏ chuỗi theo từ điển, chúng ta nên chọn các ký tự có sẵn nhỏ nhất. Sau khi chọn chúng, việc sắp xếp chúng theo thứ tự sẽ mang lại sự sắp xếp nhỏ nhất có thể. 

Toàn bộ vấn đề giảm xuống việc đếm tất cả các ký tự trong`S + T`, lấy cái đầu tiên`|S|`các ký tự theo thứ tự được sắp xếp và trả về chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số trạng thái có thể truy cập) | O(số trạng thái có thể truy cập) | Quá chậm | 
| Tối ưu | O( | S | + | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng ký tự trong cả hai chuỗi với nhau. Các giao dịch hoán đổi duy trì tần số kết hợp này, do đó, điều này thể hiện nhóm ký tự hoàn chỉnh có sẵn. 
2. Xác định rằng chuỗi cuối cùng`S`phải chứa chính xác`|S|`nhân vật. Chúng ta cần chọn nhiều nhân vật này từ nhóm kết hợp. 
3. Lặp lại bảng chữ cái từ`'a'`ĐẾN`'z'`và liên tục lấy ký tự trong khi vẫn còn vị trí cần điền vào`S`. Chọn các chữ cái nhỏ hơn trước là sự lựa chọn tham lam vì thứ tự từ điển được quyết định bởi vị trí khác biệt sớm nhất. 
4. Nối các ký tự đã chọn theo thứ tự bảng chữ cái và in kết quả. Việc sắp xếp được thực hiện một cách tự nhiên bằng cách lặp qua bảng chữ cái thay vì thu thập và sắp xếp danh sách. 

Tại sao nó hoạt động: Điều bất biến là sau khi chọn các ký tự trong bảng chữ cái theo thứ tự tăng dần, câu trả lời được xây dựng một phần luôn chứa tiền tố nhỏ nhất có thể có trong số tất cả các chuỗi cuối cùng hợp lệ. Nếu một ký tự lớn hơn được chọn trong khi tồn tại một ký tự nhỏ hơn không được sử dụng thì việc thay thế ký tự lớn hơn sẽ làm cho chuỗi nhỏ hơn ở vị trí đầu tiên nơi chúng khác nhau. Vì mỗi nhân vật trong trận chung kết`S`đến từ cùng một nhóm được bảo toàn, chọn phần nhỏ nhất`|S|`các ký tự và sắp xếp chúng ngày càng mang lại sự tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()
        t_str = input().strip()

        cnt = [0] * 26

        for c in s:
            cnt[ord(c) - ord('a')] += 1

        for c in t_str:
            cnt[ord(c) - ord('a')] += 1

        need = len(s)
        cur = []

        for i in range(26):
            take = min(cnt[i], need)
            if take:
                cur.append(chr(ord('a') + i) * take)
                need -= take
            if need == 0:
                break

        ans.append(''.join(cur))

    sys.stdout.write('\n'.join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc từng cặp chuỗi vì mỗi trường hợp thử nghiệm bao gồm một chuỗi nguồn và một chuỗi phụ. Mảng tần số có kích thước 26 vì chỉ tồn tại các chữ cái tiếng Anh viết thường. 

Hai vòng đếm kết hợp các ký tự từ cả hai chuỗi. Đây là sự chuyển đổi trung tâm của vấn đề: một khi các chuỗi được hợp nhất về mặt khái niệm, các vị trí ban đầu không còn quan trọng nữa. 

Vòng lặp xây dựng quét các ký tự từ nhỏ nhất đến lớn nhất. Giá trị được lưu trữ trong`need`theo dõi xem còn bao nhiêu nhân vật phải xếp vào bản cuối cùng`S`. sử dụng`min(cnt[i], need)`ngăn chặn việc lấy nhiều bản sao hơn mức tồn tại hoặc nhiều ký tự hơn độ dài câu trả lời yêu cầu. 

Không có vấn đề về ranh giới chỉ mục vì vòng lặp bảng chữ cái được cố định từ 0 đến 25. Số nguyên Python cũng tránh được vấn đề tràn. Câu trả lời cuối cùng được xây dựng trực tiếp theo thứ tự sắp xếp nên không cần thêm bước sắp xếp nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
ab
a
```Các ký tự kết hợp là`a`,`a`, Và`b`. trận chung kết`S`cần hai ký tự. 

| Bước | Nhân vật được xem xét | Tần số có sẵn | Nhân vật lấy | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | một | 2 | aa | 0 | 
| 2 | b | 1 | không | 0 | 

Câu trả lời là:```
aa
```Điều này chứng tỏ rằng các nhân vật từ`T`có thể trở thành một phần của`S`và vị trí ban đầu không hạn chế lựa chọn cuối cùng. 

### Ví dụ 2 

đầu vào:```
1
abd
codedigger
```Nhóm kết hợp bắt đầu bằng:```
a b c d d e e g g i o r
```trận chung kết`S`cần ba ký tự. 

| Bước | Nhân vật được xem xét | Tần số có sẵn | Nhân vật lấy | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | một | 1 | một | 2 | 
| 2 | b | 1 | b | 1 | 
| 3 | c | 1 | c | 0 | 

Câu trả lời là:```
abc
```Điều này khẳng định sự lựa chọn tham lam là lấy những chữ cái nhỏ nhất có sẵn trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | S | 
| Không gian | O(26) | Chỉ lưu trữ tần số chữ thường | 

Kích thước đầu vào tối đa là nhỏ nhưng giải pháp cũng có quy mô phù hợp với các chuỗi lớn hơn nhiều vì nó chỉ thực hiện một lượng công việc bổ sung không đổi sau khi đếm các ký tự. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            s = input().strip()
            t_str = input().strip()

            cnt = [0] * 26

            for c in s + t_str:
                cnt[ord(c) - ord('a')] += 1

            need = len(s)
            res = []

            for i in range(26):
                take = min(cnt[i], need)
                res.append(chr(i + ord('a')) * take)
                need -= take
                if need == 0:
                    break

            out.append(''.join(res))

        return '\n'.join(out)

    result = solve()
    sys.stdin = old_stdin
    return result

assert solve_io("""5
ab
a
abc
abc
abd
codedigger
dbc
a
adb
codealittle
""") == """aa
aab
abc
abc
aab""", "samples"

assert solve_io("""1
a
a
""") == "a", "single character"

assert solve_io("""1
zzz
abc
""") == "abc", "all useful characters in T"

assert solve_io("""1
aaa
aaa
""") == "aaa", "all equal values"

assert solve_io("""1
zyx
abcdefghijklmnopqrstuvw
""") == "abc", "boundary with many smaller characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab / a`|`aa`| Nhân vật có thể di chuyển từ`T`vào trong`S`| 
|`a / a`|`a`| Trường hợp kích thước tối thiểu | 
|`zzz / abc`|`abc`| Những nhân vật xuất sắc nhất đều có thể đến từ`T`| 
|`aaa / aaa`|`aaa`| Xử lý ký tự bằng nhau | 
|`zyx / abc...w`|`abc`| Sửa lỗi lựa chọn tham lam trên bảng chữ cái | 

## Vỏ cạnh 

cho`S = "ba"`Và`T = "c"`, mảng tần số chứa một`a`, một`b`, và một`c`. Thuật toán cần hai ký tự và quét bảng chữ cái. Phải mất`a`đầu tiên, sau đó`b`, sản xuất`ab`. Điều này tránh được sai lầm chỉ sắp xếp lại bản gốc`S`. 

Vì`S = "aaa"`Và`T = "aaa"`, tần số của`a`là sáu. Thuật toán lấy đúng ba bản sao vì`need`bắt đầu lúc ba giờ. Nó dừng ngay sau khi điền câu trả lời, tạo ra`aaa`mà không cần thử các giao dịch hoán đổi không cần thiết. 

Vì`S = "zzz"`Và`T = "abc"`, thuật toán thấy rằng ba ký tự nhỏ nhất trong tổng nhóm là`a`,`b`, Và`c`. Nó đặt chúng trực tiếp vào câu trả lời, chỉ ra lý do tại sao việc hoán đổi lặp đi lặp lại cho phép thay thế hoàn toàn nội dung ban đầu của`S`.
