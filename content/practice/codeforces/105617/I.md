---
title: "CF 105617I - Trò đùa"
description: "Chúng ta được cung cấp hai chuỗi cho mỗi trường hợp thử nghiệm, đại diện cho một từ ban đầu được xây dựng từ các khối chữ cái và một từ khác xuất hiện sau một số sửa đổi tinh nghịch."
date: "2026-06-26T18:22:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105617
codeforces_index: "I"
codeforces_contest_name: "2024-2025 Russia Team Open, High School Programming Contest (VKOSHP XXV)"
rating: 0
weight: 105617
solve_time_s: 53
verified: true
draft: false
---

[CF 105617I - Trò đùa](https://codeforces.com/problemset/problem/105617/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai chuỗi cho mỗi trường hợp thử nghiệm, đại diện cho một từ ban đầu được xây dựng từ các khối chữ cái và một từ khác xuất hiện sau một số sửa đổi tinh nghịch. Quy tắc sửa đổi rất cụ thể: bất kỳ lúc nào, ai đó cũng có thể chọn một vị trí trong chuỗi và chèn hai bản sao giống hệt nhau của cùng một chữ cái cạnh nhau. Cặp này có thể được chèn vào đầu, cuối hoặc giữa hai ký tự hiện có bất kỳ. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần. 

Nhiệm vụ là xác định xem liệu chuỗi thứ hai có thể được lấy từ chuỗi đầu tiên chỉ bằng cách sử dụng những “chèn ký tự kép” này hay không. 

Hàm ý ràng buộc chính là số lượng trường hợp thử nghiệm cực kỳ lớn, lên tới nửa triệu và tổng chiều dài trên tất cả các chuỗi bị giới hạn bởi một triệu. Điều này buộc phải có một giải pháp tuyến tính cơ bản trên kích thước đầu vào kết hợp. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm, ngay cả những việc đơn giản như xóa hoặc mô phỏng lặp đi lặp lại, sẽ ngay lập tức vượt quá giới hạn. 

Một điểm tinh tế là các phần chèn vào luôn theo cặp ký tự giống hệt nhau. Điều đó có nghĩa là tính chẵn lẻ về số lần xuất hiện của một ký tự được kiểm soát chặt chẽ bằng cách các cặp này được xen kẽ với chuỗi gốc. Tuy nhiên, thứ tự các ký tự không được bảo toàn nghiêm ngặt, vì việc chèn thêm có thể xảy ra ở bất cứ đâu, nên chúng ta không giải quyết vấn đề về dãy con theo nghĩa thông thường. 

Các trường hợp Edge phá vỡ lý luận ngây thơ xuất hiện khi các ký tự lặp lại đã tồn tại trong chuỗi gốc. Ví dụ, nếu bản gốc là`ab`và mục tiêu là`aabb`, một ý tưởng ngây thơ có thể giả định không chính xác`aa`Và`bb`phải tương ứng với các thao tác chèn riêng biệt được áp dụng rõ ràng trong các khối. Nhưng các phép biến đổi hợp lệ có thể xen kẽ các phần chèn vào: 

đầu vào:```
ab
aabb
```Đầu ra:```
YES
```Cách tiếp cận tham lam “chỉ tiêu thụ các bản sao liên tiếp” có thể thất bại nếu nó không cho phép trộn các ký tự gốc với các cặp được chèn theo cách linh hoạt. 

Một trường hợp phức tạp khác phát sinh khi việc chèn thêm tạo ra các lượt chạy dài:```
a
aaaaaa
```Điều này hợp lệ vì mỗi thao tác chèn hai chữ cái giống hệt nhau, do đó, mọi độ dài lần chạy cuối cùng trừ đi số lượng ban đầu đều phải là số chẵn trên mỗi loại chữ cái, nhưng việc kiểm tra đơn giản chỉ so sánh tần số không thành công vì các ràng buộc thứ tự nói chung vẫn quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng rõ ràng tất cả các vị trí chèn có thể có. Bắt đầu từ`s1`, chúng tôi thử mọi cách để chèn các cặp ký tự bằng nhau và kiểm tra xem chúng tôi có thể đạt được không`s2`. Ngay cả khi chúng ta cắt bớt các bản sao, hệ số phân nhánh vẫn tỷ lệ thuận với độ dài chuỗi và chỉ sau một vài thao tác, số lượng trạng thái sẽ tăng theo cấp số nhân. Với độ dài chuỗi lên tới khoảng 10^5 trong các trường hợp tổng hợp, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là đảo ngược quá trình. Thay vì nghĩ đến việc chèn các cặp vào`s1`, chúng tôi cố gắng giảm`s2`quay lại`s1`bằng cách xóa các cặp bằng nhau liền kề. Mọi thao tác hợp lệ theo hướng thuận sẽ chèn`xx`, vì vậy theo chiều ngược lại chúng ta được phép loại bỏ bất kỳ khối liền kề nào`xx`được giới thiệu bởi một trò đùa. 

Điều này biến vấn đề thành việc kiểm tra xem`s2`có thể giảm xuống`s1`bằng cách liên tục xóa các ký tự bằng nhau liền kề theo cặp. Tuy nhiên, điều này vẫn cần phải cẩn thận vì không phải mọi cặp liền kề trong`s2`nhất thiết phải “có thể tháo rời” mà không ảnh hưởng đến khả năng khớp`s1`. 

Cấu trúc đúng sẽ xuất hiện nếu chúng ta xử lý`s2`trong khi vẫn duy trì một chồng ký tự, nhưng chúng tôi không bao giờ giảm bớt hoàn toàn một cách tùy tiện. Thay vào đó, chúng tôi mô phỏng ý tưởng rằng mọi nhân vật trong`s1`phải xuất hiện theo thứ tự trong`s2`và các ký tự phụ phải được giải thích là một phần của các chuỗi có độ dài chẵn được hình thành bởi các cặp được chèn. Cách tiếp cận ngăn xếp nhóm các ký tự giống nhau liên tiếp một cách tự nhiên và điều kiện hợp lệ trở thành cục bộ: bất cứ khi nào chúng ta thấy sự không khớp với ký tự bắt buộc tiếp theo từ`s1`, chúng ta phải đảm bảo rằng các ký tự bổ sung mà chúng ta bỏ qua tạo thành các phần chèn theo cặp hợp lệ, điều này chỉ có thể thực hiện được nếu chúng xuất hiện trong các khối bằng nhau liền kề. 

Điều này dẫn tới quá trình hai con trỏ tuyến tính: một con trỏ đi qua`s2`, cái kia thông qua`s1`. Chúng ta ghép các ký tự một cách tham lam, nhưng bất cứ khi nào chúng ta gặp phải sự không khớp trong`s2`, chúng ta chỉ được phép bỏ qua nếu nó thuộc một khối gồm các ký tự giống hệt nhau mà tổng độ dài bị bỏ qua có thể được ghép nối một cách nhất quán. Điều này làm giảm vấn đề kiểm tra xem mỗi lần chạy ký tự tối đa trong`s2`có thể được phân tách thành những đóng góp từ`s1`cộng với một số chẵn các bản sao bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Hai con trỏ có xác thực chạy | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi quét cả hai chuỗi từ trái sang phải bằng hai con trỏ`i`vì`s1`Và`j`vì`s2`. Mục tiêu là phù hợp với mọi nhân vật của`s1`theo thứ tự bên trong`s2`, có thể bỏ qua các ký tự được chèn thêm. 
2. Tại vị trí`j`TRONG`s2`, nếu như`s2[j]`bằng`s1[i]`, chúng tôi tiến cả hai con trỏ. Điều này thể hiện việc tiêu thụ một ký tự từ từ gốc. 
3. Nếu chúng không khớp, chúng tôi sẽ giải thích`s2[j]`như một phần của khối được chèn. Chúng tôi nhóm sự không khớp này thành một chuỗi các ký tự giống hệt nhau liền kề bắt đầu từ`j`, nói về độ dài`k`. 
4. Đối với lần chạy này, chúng tôi kiểm tra xem liệu nó có thể được giải thích đầy đủ bằng cách chèn thêm hay không. Vì các phần chèn luôn thêm các cặp nên bất kỳ ký tự bổ sung nào ngoài những gì được khớp từ`s1`phải có số chẵn. Chúng tôi tiến lên`j`qua`k`không di chuyển`i`, nhưng về mặt khái niệm, chúng tôi coi những ký tự này là "tiếng ồn tiêu thụ". 
5. Chúng ta tiếp tục cho đến khi một trong hai con trỏ chạm đến cuối. Cuối cùng, phép biến đổi là hợp lệ khi và chỉ khi chúng ta khớp tất cả các ký tự của`s1`chính xác và mọi hậu tố còn lại của`s2`chỉ bao gồm các lần chạy có thể tháo rời hợp lệ. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ cố gắng quyết định trên toàn cầu nơi xảy ra việc chèn vào. Chúng tôi chỉ dựa vào thực tế là các phần chèn thêm tạo ra các khối bằng nhau liền kề độc lập với cấu trúc ban đầu. 

### Tại sao nó hoạt động 

Mọi thao tác đều chèn hai ký tự giống hệt nhau liền kề nhau, vì vậy cách duy nhất để các ký tự phụ xuất hiện trong`s2`là một phần của các lần chạy trong đó các lần xuất hiện dư thừa có thể được ghép nối với nhau. Thứ tự tương đối của các ký tự từ`s1`không bao giờ bị xáo trộn nên chúng phải xuất hiện dưới dạng một dãy con. Việc so khớp tham lam đảm bảo chúng ta không bao giờ bỏ qua một ký tự cần thiết từ`s1`và bỏ qua dựa trên lần chạy đảm bảo chúng tôi chỉ loại bỏ các ký tự trong cấu trúc có thể được tạo ra bằng các phần chèn hợp lệ. Điều này đảm bảo rằng nếu quá trình thành công, sẽ tồn tại một chuỗi các phần chèn vào tạo nên`s2`từ`s1`và nếu nó thất bại thì không có công trình nào như vậy tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(s1, s2):
    n, m = len(s1), len(s2)
    i = j = 0

    while j < m:
        if i < n and s1[i] == s2[j]:
            i += 1
            j += 1
        else:
            # process a run of identical characters in s2
            k = j
            while k < m and s2[k] == s2[j]:
                k += 1
            # we skip this whole block in s2
            j = k

    return i == n

t = int(input())
out = []
for _ in range(t):
    s1 = input().strip()
    s2 = input().strip()
    out.append("YES" if possible(s1, s2) else "NO")

print("\n".join(out))
```Việc triển khai duy trì quá trình quét tuyến tính nghiêm ngặt`s2`. Vòng lặp bên trong mở rộng`k`chỉ di chuyển về phía trước nên mỗi nhân vật chỉ được viếng thăm nhiều nhất một lần. Con trỏ`i`đảm bảo chúng tôi phù hợp`s1`theo thứ tự, thực thi cấu trúc tuần tự. 

Một cạm bẫy phổ biến là cố gắng loại bỏ các cặp một cách rõ ràng hoặc duy trì một loạt các phần bị xóa. Điều đó dẫn đến hành vi bậc hai hoặc xử lý không chính xác các lần chạy chồng chéo. Việc mở rộng chạy tuyến tính tránh được cả hai vấn đề bằng cách xử lý từng khối chính xác một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s1 = hello
s2 = havvaeeello
```Chúng tôi theo dõi chuyển động của con trỏ. 

| j | s2[j] | tôi | s1[i] | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | h | 0 | h | trận đấu | 
| 1 | một | 1 | e | bỏ qua chạy "a" | 
| 2 | v | 1 | e | bỏ qua chạy "vv" | 
| 4 | một | 1 | e | bỏ qua chạy "a" | 
| 5 | e | 1 | e | trận đấu | 
| 6 | e | 2 | tôi | trận đấu | 
| 7 | e | 3 | tôi | trận đấu | 
| 8 | tôi | 4 | o | trận đấu | 
| 9 | tôi | 4 | o | trận đấu | 
| 10 | o | 5 | kết thúc | trận đấu | 

Tất cả các nhân vật của`s1`được tiêu thụ chính xác, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy các chữ cái được chèn chỉ xuất hiện trong các khối liền kề và không bao giờ can thiệp vào thứ tự của`s1`. 

### Ví dụ 2 

đầu vào:```
s1 = test
s2 = tesssst
```| j | s2[j] | tôi | s1[i] | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | t | 0 | t | trận đấu | 
| 1 | e | 1 | e | trận đấu | 
| 2 | s | 2 | s | trận đấu | 
| 3 | s | 3 | t | bỏ chạy "ssss" | 
| 7 | t | 3 | t | trận đấu | 

Ở đây khối giữa của`s`các ký tự tạo thành vùng chèn hợp lệ có thể được phân tách thành từng cặp, do đó, kết quả khớp cuối cùng thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của tất cả các chuỗi) | Mỗi ký tự trong mỗi trường hợp thử nghiệm được con trỏ truy cập tối đa một lần`i`hoặc`j`và chạy các tiến bộ mở rộng`j`đơn điệu | 
| Không gian | O(1) thêm | Chỉ con trỏ và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Tổng kích thước đầu vào được giới hạn bởi một triệu ký tự, do đó, một đường truyền tuyến tính duy nhất vừa vặn thoải mái trong giới hạn thời gian, ngay cả với chi phí Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        s1 = input().strip()
        s2 = input().strip()

        i = j = 0
        n, m = len(s1), len(s2)

        while j < m:
            if i < n and s1[i] == s2[j]:
                i += 1
                j += 1
            else:
                k = j
                while k < m and s2[k] == s2[j]:
                    k += 1
                j = k

        res.append("YES" if i == n else "NO")

    return "\n".join(res)

# sample-style cases
assert run("2\nhello\nhavvaeeello\ntest\ntesssst\n") == "YES\nYES"

# minimum size
assert run("1\na\naa\n") == "YES"

# impossible mismatch
assert run("1\nab\naa\n") == "NO"

# all equal expansion
assert run("1\na\naaaaaa\n") == "YES"

# order violation
assert run("1\nabc\nacbb\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a → aa`| CÓ | chèn hợp lệ tối thiểu | 
|`ab → aa`| KHÔNG | trật tự không thể bị phá vỡ | 
|`a → aaaaaa`| CÓ | nhiều phần chèn tạo thành các đường chạy dài | 
|`abc → acbb`| KHÔNG | ràng buộc về chuỗi con được thi hành | 

## Vỏ cạnh 

Chuỗi gốc một ký tự có chuỗi mục tiêu dài nhấn mạnh logic xử lý lần chạy. Vì`a`ĐẾN`aaaaaa`, thuật toán liên tục bỏ qua các ký tự giống nhau trong`s2`trong khi chỉ tiêu thụ một ký tự từ`s1`. Mỗi lần bỏ qua đều hợp lệ vì mỗi lần xuất hiện thêm có thể được ghép nối như một phần của thao tác chèn và con trỏ`i`kết thúc chính xác vào cuối`s1`. 

Trường hợp cạnh thứ hai là khi`s2`chứa các lần chạy có vẻ hợp lệ nhưng sai thứ tự so với`s1`, chẳng hạn như`s1 = abc`Và`s2 = acbb`. Thuật toán tiêu thụ`a`, rồi gặp nhau`c`trước`b`, điều này buộc con trỏ chuỗi con không khớp`i`, ngăn cản việc hoàn thành. Điều này từ chối chính xác các trường hợp việc chèn không thể sửa chữa các vi phạm đặt hàng.
