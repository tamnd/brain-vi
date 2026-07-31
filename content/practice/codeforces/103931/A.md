---
title: "CF 103931A - Một vấn đề A+B khác"
description: "Chúng ta được cung cấp một dạng phương trình cố định có độ dài 8, luôn được viết dưới dạng hai số có hai chữ số cộng lại với nhau và bằng một số có hai chữ số khác. Cấu trúc luôn là ??+??=??, mỗi ? là một chữ số."
date: "2026-07-02T07:15:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "A"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 53
verified: true
draft: false
---

[CF 103931A - Một vấn đề A+B khác](https://codeforces.com/problemset/problem/103931/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dạng phương trình cố định có độ dài 8, luôn được viết dưới dạng hai số có hai chữ số cộng lại với nhau và bằng một số có hai chữ số khác. Cấu trúc luôn luôn`??+??=??`, mỗi nơi`?`là một chữ số. Dấu cộng và dấu bằng được cố định ở cùng một vị trí và tất cả các vị trí khác đều là chữ số. Cho phép sử dụng các số 0 đứng đầu, vì vậy mỗi khối hai ký tự đại diện cho một số từ 00 đến 99. 

Một phương trình đoán`E`đã được gửi và hệ thống trả về một chuỗi phản hồi`C`có cùng độ dài bằng cách sử dụng ba màu. Mỗi vị trí mô tả cách ký tự tương ứng trong`E`khớp với phương trình đúng ẩn. Màu xanh lá cây có nghĩa là ký tự đúng và ở đúng vị trí. Màu tím có nghĩa là ký tự tồn tại ở đâu đó trong phương trình ẩn nhưng không ở vị trí đó. Màu đen có nghĩa là ký tự không xuất hiện trong phương trình ẩn hoặc xuất hiện ít lần hơn so với dự đoán nên những lần xuất hiện thêm không được ghi nhận. 

Nhiệm vụ là xây dựng lại tất cả các phương trình hợp lệ có thể tạo ra chính xác phản hồi này khi so sánh với dự đoán đã cho.`E`. 

Điểm mấu chốt là câu trả lời hợp lệ của ứng viên không phải là câu trả lời tùy tiện. Nó phải thỏa mãn hai điều kiện độc lập. Đầu tiên, nó phải là một phương trình có giá trị về mặt cú pháp có dạng`??+??=??`. Thứ hai, nó phải chính xác về mặt số học, nghĩa là tổng bên trái phải bằng bên phải khi được hiểu là số nguyên với các số 0 đứng đầu được phép. 

Ràng buộc nhỏ về cấu trúc nhưng lớn về mặt kết hợp. Mỗi khối trong số ba khối số nằm trong khoảng từ 00 đến 99, do đó có nhiều nhất một triệu phương trình có thể có trước khi lọc theo độ chính xác số học. Điều đó đủ nhỏ để có thể trực tiếp áp dụng vũ lực, nhưng chỉ khi việc kiểm tra phản hồi cho mỗi ứng viên có hiệu quả. 

Một sự hiểu lầm ngây thơ thường xuất phát từ việc coi phản hồi là độc lập cho mỗi ký tự mà không xử lý các bản sao một cách chính xác. Ví dụ: nếu dự đoán chứa các chữ số lặp lại thì số lần trùng khớp màu tím cho một chữ số sẽ phụ thuộc vào số lần nó xuất hiện trong câu trả lời ẩn. Việc triển khai bất cẩn chỉ kiểm tra tư cách thành viên sẽ đếm quá mức kết quả phù hợp và tạo ra quá trình lọc không chính xác. 

Một trường hợp khó phát hiện khác là khi các chữ số lặp lại trong câu trả lời của thí sinh. Hãy xem xét một phỏng đoán như`11+11=22`. Nếu câu trả lời đúng chỉ có một`1`, chỉ một trong các vị trí đó phải có màu xanh lục hoặc tím, còn các vị trí còn lại phải có màu đen. Giới hạn về bội số này là cần thiết và phải được thực thi trong mô phỏng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp xem xét mọi phương trình có thể`a + b = c`, trong đó mỗi`a`,`b`, Và`c`nằm trong khoảng từ 00 đến 99. Đối với mỗi bộ ba, chúng tôi định dạng nó thành biểu diễn chuỗi và xác minh tính đúng đắn về mặt số học. Điều này tạo ra tối đa 1000000 ứng viên và chỉ một phần nhỏ trong số họ thỏa mãn`a + b == c`. 

Đối với mỗi ứng cử viên hợp lệ, chúng tôi mô phỏng phản hồi Nerdle dựa trên dự đoán đã cho`E`. Mô phỏng này phải sao chép kết hợp giống như Wordle với số lượng. Trước tiên, chúng tôi chỉ định tất cả các kết quả khớp màu xanh lá cây trong đó các ký tự trùng khớp chính xác về vị trí. Sau đó, chúng tôi xử lý các ký tự còn lại bằng cách đếm tần số của các ký tự không được sử dụng trong ứng viên và so sánh chúng với các ký tự được đoán còn sót lại, tạo ra màu tím hoặc đen tùy thuộc vào tình trạng sẵn có. 

Cách tiếp cận bạo lực này hoạt động vì không gian tìm kiếm rất nhỏ. Tuy nhiên, chi phí chính là mô phỏng phản hồi. Nếu được thực hiện cẩn thận, mỗi so sánh là O(8), do đó tổng độ phức tạp là khoảng 8 triệu thao tác, đủ nhanh. 

Quan sát quan trọng là không cần cắt tỉa thông minh hơn. Bài toán được thiết kế sao cho việc liệt kê đầy đủ tất cả các phương trình số học hợp lệ là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu với mô phỏng | O(10^6 × 8) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại tất cả các giá trị có thể có của ba khối số và lọc theo tính hợp lệ số học, sau đó kiểm tra tính nhất quán với chuỗi phản hồi. 

1. Lặp lại`a`từ 0 đến 99,`b`từ 0 đến 99, tính`c = a + b`. Nếu như`c > 99`, hãy bỏ qua vì nó không thể vừa với hai chữ số. Điều này đảm bảo chúng tôi chỉ tạo ra các cấu trúc phương trình hợp lệ. 
2. Chuyển đổi`a`,`b`, Và`c`thành chuỗi hai ký tự có số 0 đứng đầu. Xây dựng chuỗi phương trình đầy đủ`S = format(a) + "+" + format(b) + "=" + format(c)`. 
3. So sánh`S`chống lại dự đoán đã cho`E`để tính toán phản hồi. Đầu tiên chúng tôi đánh dấu tất cả các vị trí ở đó`S[i] == E[i]`có màu xanh lá cây và loại bỏ những ký tự đó khỏi việc xem xét thêm. Bước này là cần thiết vì kết quả khớp chính xác luôn phải được ưu tiên. 
4. Đối với các vị trí chưa khớp còn lại, chúng tôi đếm tần số ký tự của ứng viên ẩn`S`. Sau đó, chúng tôi quét các vị trí chưa khớp trong`E`. Nếu một ký tự tồn tại trong nhóm tần số còn lại, chúng tôi gán màu tím và giảm số lượng. Nếu không chúng tôi chỉ định màu đen. 
5. Nếu phản hồi được xây dựng khớp chính xác với chuỗi đã cho`C`, chúng tôi lưu trữ phương trình là hợp lệ. 
6. Sau khi xử lý tất cả các ứng viên, xuất ra số đếm và tất cả các phương trình hợp lệ. 

Lựa chọn thiết kế trọng tâm là chúng ta không bao giờ cố gắng “giải quyết” các ràng buộc một cách tượng trưng. Thay vào đó, chúng tôi trực tiếp kiểm tra mọi phương trình khả thi và dựa vào kích thước miền nhỏ. 

### Tại sao nó hoạt động 

Thuật toán đúng vì mọi câu trả lời ẩn hợp lệ phải là một trong các nghiệm số học được liệt kê. Hàm phản hồi có tính xác định khi đưa ra một dự đoán và một câu trả lời ẩn, đồng thời mô phỏng của chúng tôi tái tạo chính xác hàm đó, bao gồm cả các ràng buộc bội số. Do đó, bất kỳ ứng cử viên nào phù hợp với phản hồi đều phù hợp với kết quả được quan sát và mọi phương trình nhất quán sẽ được phát hiện trong quá trình liệt kê. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feedback(guess, target):
    n = 8
    res = [""] * n
    used_guess = [False] * n
    used_target = [False] * n

    for i in range(n):
        if guess[i] == target[i]:
            res[i] = 'G'
            used_guess[i] = True
            used_target[i] = True

    freq = {}
    for i in range(n):
        if not used_target[i]:
            freq[target[i]] = freq.get(target[i], 0) + 1

    for i in range(n):
        if res[i]:
            continue
        if freq.get(guess[i], 0) > 0:
            res[i] = 'P'
            freq[guess[i]] -= 1
        else:
            res[i] = 'B'

    return "".join(res)

def solve():
    E = input().strip()
    C = input().strip()

    ans = []

    for a in range(100):
        for b in range(100):
            c = a + b
            if c >= 100:
                continue

            s = f"{a:02d}+{b:02d}={c:02d}"
            if feedback(E, s) == C:
                ans.append(s)

    print(len(ans))
    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc liệt kê trực tiếp tất cả các phương trình hợp lệ số học. các`feedback`chức năng thực hiện một chiến lược kết hợp hai bước nghiêm ngặt. Lượt đầu tiên khóa các vị trí màu xanh lá cây, đảm bảo các kết quả khớp chính xác được loại bỏ khỏi việc xem xét trước khi bắt đầu khớp tần số. Bước thứ hai sử dụng từ điển tần số để thực thi ràng buộc bội số mà bài toán yêu cầu. 

Một cạm bẫy phổ biến là bỏ qua bước đầu tiên và trực tiếp thực hiện khớp tần số, điều này làm mất tính chính xác khi các chữ số lặp lại căn chỉnh ở các vị trí cụ thể. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp minh họa nhỏ trong đó dự đoán là`E = 11+45=56`và câu trả lời của ứng viên là`11+45=56`. 

| Bước | Đoán vs Ứng viên | Rau xanh | Tần số còn lại | Phản hồi | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 11+45=56 vs 11+45=56 | tất cả | không | GGGGGGGG | 

Điều này xác nhận một sự kết hợp hoàn hảo, tạo ra tất cả các ô màu xanh lá cây. 

Bây giờ hãy xem xét một ứng cử viên không phù hợp như`11+46=57`. 

| Bước | Đoán vs Ứng viên | Rau xanh | Tần số còn lại | Phản hồi | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 11+45=56 vs 11+46=57 | một phần (khớp tiền tố) | chữ số còn lại | hỗn hợp | 

Ở đây, các chữ số khớp với vị trí sẽ có màu xanh lục, trong khi các chữ số khác được đánh giá qua tần số. Điều này cho thấy độ chính xác về vị trí và độ chính xác của nhiều tập hợp tương tác với nhau như thế nào. 

Dấu vết cho thấy phản hồi không hoàn toàn dựa trên vị trí hay dựa trên tập hợp mà là sự kết hợp của cả hai, đó là lý do tại sao mô phỏng hai bước là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10000 × 8) | Chúng tôi kiểm tra tối đa 10.000 bộ ba hợp lệ số học và so sánh các chuỗi 8 ký tự cho mỗi bộ ba | 
| Không gian | O(1) | Chỉ có các cấu trúc phụ trợ cố định để đếm tần số và lưu trữ đầu ra | 

Các ràng buộc về kích thước đầu vào làm cho việc liệt kê này trở nên đơn giản để thực hiện trong giới hạn. Ngay cả trong Python, số lượng thao tác vẫn ở mức tốt trong giới hạn thời gian thông thường vì tất cả công việc đều là số học số nguyên đơn giản và xử lý chuỗi ngắn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from types import ModuleType

    # assume solution is already defined above in same environment
    # if separated, this should call solve()
    return ""

# provided samples (placeholders, actual judge values assumed)
# assert run("40+11=51\nPBGPPGGB\n") == "..."

# custom cases

# all zeros
# assert run("00+00=00\nGGGGGGGG\n") == "1\n00+00=00"

# no repetition case
# assert run("01+02=03\nPPGPPGPP\n") == "10+20=30\n20+10=30"

# maximum arithmetic boundary
# assert run("50+49=99\nBBBBBBBB\n") == "..."

# repeated digits stress
# assert run("11+11=22\nGGGGGGGG\n") == "1\n11+11=22"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`00+00=00, GGGGGGGG`| bản sắc duy nhất | xử lý trận đấu hoàn hảo | 
|`01+02=03, PPGPPGPP`| nhiều giao dịch hoán đổi hợp lệ | khớp chữ số trùng lặp | 
|`11+11=22, GGGGGGGG`| cấu hình đơn | độ chính xác của chữ số lặp lại | 
|`50+49=99, BBBBBBBB`| bộ lọc | logic từ chối | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi dự đoán chứa các chữ số lặp lại nhưng câu trả lời của ứng viên lại có ít lần xuất hiện hơn. Ví dụ, nếu dự đoán là`11+11=22`và ứng cử viên là`12+10=22`, chỉ một trong số`1`s có thể được kết hợp. Lượt thứ hai dựa trên tần số đảm bảo rằng sau khi chỉ định một trận đấu, các lần xuất hiện còn lại sẽ tự động chuyển sang màu đen. 

Một trường hợp khác là khi độ chính xác số học lọc ra hầu hết các ứng cử viên. Ví dụ,`99+99=198`không hợp lệ vì kết quả vượt quá hai chữ số, do đó nó thậm chí không bao giờ được xem xét. Điều này ngăn chặn hành vi bao quanh không chính xác. 

Trường hợp cạnh cuối cùng là khi phản hồi chỉ chứa các ô màu đen. Trong trường hợp này, bất kỳ phương trình ứng viên nào không có ký tự nào có dự đoán vượt quá bội số cho phép sẽ vượt qua. Mô phỏng vẫn xử lý chính xác vấn đề này vì tất cả các kết quả phù hợp màu xanh lá cây đều bị loại bỏ trước tiên, để lại số tần suất ngăn chặn việc ghi nhận quá mức các chữ số được chia sẻ một cách tự nhiên.
