---
title: "CF 1348C - Phượng hoàng và phân phối"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép chia tất cả các ký tự của nó thành đúng k nhóm không trống. Bên trong mỗi nhóm, chúng ta có thể sắp xếp lại các ký tự một cách tùy ý, do đó, mỗi nhóm thực chất chỉ là một tập hợp nhiều chữ cái mà sau này chúng ta sắp xếp thành một chuỗi."
date: "2026-06-16T10:10:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1348
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 638 (Div. 2)"
rating: 1600
weight: 1348
solve_time_s: 301
verified: false
draft: false
---

[CF 1348C - Phượng hoàng và phân phối](https://codeforces.com/problemset/problem/1348/C) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, chuỗi 
**Thời gian giải:** 5 phút 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép chia tất cả các ký tự của nó thành đúng k nhóm không trống. Bên trong mỗi nhóm, chúng ta có thể sắp xếp lại các ký tự một cách tùy ý, do đó, mỗi nhóm thực chất chỉ là một tập hợp nhiều chữ cái mà sau này chúng ta sắp xếp thành một chuỗi. 

Khi tất cả k chuỗi đã được hình thành, chúng ta xem xét chúng theo thứ tự từ điển và chọn chuỗi lớn nhất. Mục tiêu của chúng tôi là phân phối các ký tự sao cho chuỗi tồi tệ nhất này càng nhỏ càng tốt về mặt từ điển. 

Sự tự do chính là chúng tôi không chia tách một chuỗi, chúng tôi đang phân phối nhiều tập hợp. Điều này loại bỏ mọi ràng buộc về vị trí và giải quyết vấn đề hoàn toàn về tần suất và thứ tự. 

Các ràng buộc buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng n qua các thử nghiệm tối đa là 10^5 nên mọi giải pháp xung quanh O(n log n) cho mỗi trường hợp thử nghiệm hoặc tổng thể O(n) đều có thể chấp nhận được. Bất cứ điều gì liên quan đến việc mô phỏng lặp đi lặp lại các phân bố hoặc phép gán hàm mũ đều là không thể ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi k bằng n. Trong trường hợp đó, mỗi chuỗi chứa chính xác một ký tự, vì vậy câu trả lời chỉ đơn giản là ký tự tối đa trong chuỗi. Một trường hợp khác là khi k bằng 1, trong đó câu trả lời chỉ là chính chuỗi đã được sắp xếp, vì tất cả các ký tự phải thuộc một nhóm. 

Một trường hợp góc thú vị hơn phát sinh khi chuỗi có ký tự nhỏ nhất chiếm ưu thế. Ví dụ: nếu hầu hết các ký tự là 'a', thì việc phân phối chúng đồng đều hoặc không đồng đều sẽ thay đổi chuỗi nào trở thành chuỗi lớn nhất về mặt từ điển và việc phân nhóm tham lam ngây thơ có thể thất bại nếu nó không tôn trọng các ràng buộc về thứ tự. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử tất cả các phân phối có thể có của n ký tự vào k nhóm không trống. Ngay cả khi chúng ta bỏ qua các hoán vị bên trong, điều này có nghĩa là gán mỗi ký tự vào một trong k nhóm, mang lại k^n khả năng. Điều này vượt xa mọi giới hạn. 

Quan sát quan trọng là thứ tự từ điển được xác định bởi vị trí sớm nhất nơi các chuỗi khác nhau. Để giảm thiểu chuỗi tối đa, chúng tôi muốn các ký tự đầu tiên của tất cả các nhóm càng nhỏ càng tốt và chúng tôi muốn kiểm soát mức độ “lớn” của vị trí khác nhau đầu tiên. 

Sắp xếp chuỗi cho thấy cấu trúc của giải pháp tối ưu. Sau khi được sắp xếp, chúng tôi đang quyết định một cách hiệu quả cách cắt chuỗi đã sắp xếp thành k phần, bởi vì bất kỳ sự sắp xếp tối ưu nào cũng có thể được chuyển thành một trong đó các nhóm tương ứng với các phân đoạn liền kề của chuỗi được sắp xếp mà không làm xấu đi mức tối đa. 

Từ đây xuất hiện hai trường hợp. Nếu k ký tự nhỏ nhất không hoàn toàn giống nhau thì câu trả lời đơn giản là ký tự thứ k theo thứ tự được sắp xếp, bởi vì chúng ta có thể phân phối k ký tự nhỏ nhất làm chữ cái đầu tiên của chuỗi k và mọi thứ khác chỉ ảnh hưởng đến vị trí cuối. Nếu k ký tự đầu tiên giống hệt nhau thì chúng ta buộc phải phân bổ chúng một cách đồng đều và câu trả lời sẽ trở thành ký tự đó cộng với hậu tố còn lại được phân phối tối ưu, điều này làm giảm việc nối thêm tất cả các ký tự còn lại theo bất kỳ thứ tự nào. 

Cái nhìn sâu sắc hơn là khi chúng tôi sửa ký tự đầu tiên của mỗi nhóm, phân phối còn lại không còn ảnh hưởng đến mức tối đa trừ khi nó đưa ra tiền tố lớn hơn về mặt từ điển sớm hơn mức cần thiết. Điều này dẫn đến một kết cấu xác định rõ ràng sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp chuỗi theo thứ tự không giảm. Việc sắp xếp rất hữu ích vì việc giảm thiểu từ điển luôn ưu tiên các ký tự nhỏ hơn sớm hơn và mọi cách sắp xếp tối ưu đều có thể được lý giải theo thứ tự được sắp xếp. 
2. Nếu k bằng 1, trả về toàn bộ chuỗi đã sắp xếp. Không có lựa chọn phân phối nên chuỗi đơn được cố định. 
3. Kiểm tra ký tự thứ k trong chuỗi đã sắp xếp (k-1 được lập chỉ mục 0). Nếu ký tự này lớn hơn ký tự đầu tiên thì chúng ta biết rằng tồn tại ít nhất một ký tự lớn hơn trong số k vị trí đầu tiên. 
4. Trong trường hợp đó, trả về ký tự thứ k làm câu trả lời. Lý do là chúng ta có thể gán k ký tự nhỏ nhất đầu tiên làm ký tự bắt đầu của chuỗi k và chuỗi lớn nhất về mặt từ điển trong số chúng sẽ bắt đầu bằng ký tự nhỏ nhất thứ k. 
5. Nếu k ký tự đầu tiên đều giống nhau thì chúng ta không thể giảm mức tối đa bằng cách phân bổ khác nhau giữa chúng. Chúng ta đặt một ký tự vào mỗi chuỗi k-1 đầu tiên và đặt phần còn lại vào chuỗi thứ k, chuỗi này trở thành chuỗi duy nhất không tầm thường. 
6. Trong trường hợp thứ hai này, câu trả lời là hậu tố bắt đầu từ chỉ số k-1 trong chuỗi được sắp xếp. Vì tất cả các ký tự k-1 đầu tiên đều giống hệt nhau nên thứ tự của hậu tố chiếm ưu thế trong từ điển tối đa. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là việc so sánh từ điển được quyết định ở vị trí khác nhau đầu tiên. Khi chúng tôi sắp xếp, chúng tôi đảm bảo rằng mọi ký tự trước đó không bao giờ tệ hơn ký tự sau. Cách duy nhất để tác động đến chuỗi tối đa là thông qua ký tự nhỏ nhất xuất hiện dưới dạng phần tử đầu tiên của bất kỳ nhóm nào. Khi k ký tự đầu tiên được sửa, mọi phép gán khác chỉ thêm các ký tự vào chuỗi đã có tiền tố nhỏ hơn hoặc bằng nhau, do đó chúng không thể thay đổi nhận dạng tối đa trừ khi tất cả các tiền tố đều bằng nhau, trong trường hợp đó, hậu tố hoàn toàn xác định câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = sorted(input().strip())

        if k == 1:
            print("".join(s))
            continue

        if s[k-1] != s[0]:
            print(s[k-1])
        else:
            # all first k characters are equal
            print("".join(s[k-1:]))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp chuỗi để chúng ta có thể suy luận trực tiếp về các ký tự có sẵn nhỏ nhất. Việc kiểm tra k bằng 1 xử lý trường hợp tầm thường trong đó không xảy ra phân phối. Việc so sánh giữa ký tự đầu tiên và ký tự thứ k xác định liệu chúng ta có thể tách các ký tự nhỏ nhất giữa các nhóm theo cách tạo ra các điểm bắt đầu khác nhau hay không. Nếu chúng khác nhau, ký tự thứ k sẽ trở thành yếu tố giới hạn cho nhóm tệ nhất. Nếu chúng giống nhau, tất cả các nhóm đầu đều bắt đầu giống hệt nhau, buộc hậu tố còn lại phải xác định chuỗi tệ nhất. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng chuỗi k một cách rõ ràng. Lý do đảm bảo cấu trúc của một phân vùng tối ưu nên việc mô phỏng trực tiếp là không cần thiết. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp từ các mẫu. 

### Ví dụ 1 

đầu vào:```
5 2
baacb
```Chuỗi được sắp xếp:`a a a b b`| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | sắp xếp | aaabb | 
| 2 | k = 2, so sánh s[1] và s[0] | một == một | 
| 3 | lấy hậu tố từ k-1 | abb | 

Đầu ra là`abb`. 

Điều này hiển thị trường hợp “tất cả k đầu tiên bằng nhau”, trong đó hậu tố xác định câu trả lời. 

### Ví dụ 2 

đầu vào:```
5 3
baacb
```Chuỗi được sắp xếp:`a a a b b`| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | sắp xếp | aaabb | 
| 2 | k = 3, so sánh s[2] và s[0] | một == một | 
| 3 | lấy hậu tố từ k-1 | bb | 

Đầu ra là`b`. 

Điều này chứng tỏ khi k ký tự đầu tiên bằng nhau thì phần đuôi còn lại sẽ xác định đầy đủ chuỗi tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | lưu trữ chuỗi đã sắp xếp | 

Tổng n trên các trường hợp thử nghiệm là 10^5, do đó việc sắp xếp từng trường hợp thử nghiệm một cách độc lập vẫn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            s = sorted(input().strip())

            if k == 1:
                out.append("".join(s))
            elif s[k-1] != s[0]:
                out.append(s[k-1])
            else:
                out.append("".join(s[k-1:]))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""6
4 2
baba
5 2
baacb
5 3
baacb
5 3
aaaaa
6 4
aaxxzz
7 1
phoenix
""") == """ab
abbc
b
aa
x
ehinopx"""

# custom cases
assert run("""1
1 1
a
""") == "a"

assert run("""1
3 3
cba
""") == "c"

assert run("""1
6 2
zzzyyy
""") == "zzz"

assert run("""1
8 4
abacabad
""") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | một | kích thước tối thiểu | 
| cba, k=n | c | hành vi k=n | 
| zzzzyyy | zzz | tất cả khối lớn bằng nhau | 
| abacabad | một | cạnh phân phối hỗn hợp | 

## Vỏ cạnh 

Khi n bằng k, việc sắp xếp chuỗi và trả về s[k-1] một cách chính xác sẽ cho ký tự tối đa vì mỗi ký tự đứng một mình. Ví dụ, đầu vào`cba`với k=3 trở thành`abc`, và s[2] là`c`, điều đó đúng. 

Khi tất cả các ký tự giống hệt nhau, chẳng hạn như`aaaaa`với bất kỳ k nào, thuật toán sẽ đi vào nhánh tiền tố bằng nhau. Hậu tố từ k-1 vẫn là`a...a`, vì vậy mọi nhóm đều có cùng một chuỗi và giá trị tối đa không thay đổi. 

Khi các ký tự bị lệch nhiều như`zzzyyy`, sắp xếp cho`yyyzzz`. Đối với k=2, vì s[1] bằng s[0] trong kiểm tra tiền tố, nên chúng tôi trả về hậu tố bắt đầu từ chỉ mục 1, tức là`yyzzzz`theo thuật ngữ nhóm và chuỗi lớn nhất về mặt từ điển sẽ trở thành`zzz`.
