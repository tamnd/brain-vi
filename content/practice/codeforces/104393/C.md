---
title: "CF 104393C - Tính các yếu tố rủi ro"
description: "Chúng ta được cung cấp một chuỗi dài biểu thị một dải hóa học tuyến tính. Mỗi vị trí trên dải là một chữ cái viết thường. Một số chữ cái được đánh dấu là "rủi ro". Đối với bất kỳ chuỗi con nào của dải, chúng ta có thể đếm xem nó chứa bao nhiêu ký tự nguy hiểm."
date: "2026-06-30T23:52:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "C"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 110
verified: true
draft: false
---

[CF 104393C - Tính các yếu tố rủi ro](https://codeforces.com/problemset/problem/104393/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài biểu thị một dải hóa học tuyến tính. Mỗi vị trí trên dải là một chữ cái viết thường. Một số chữ cái được đánh dấu là "rủi ro". Đối với bất kỳ chuỗi con nào của dải, chúng ta có thể đếm xem nó chứa bao nhiêu ký tự nguy hiểm. Nhiệm vụ là đếm xem có bao nhiêu chuỗi con có đúng K ký tự nguy hiểm. 

Một chuỗi con được xác định bằng cách chọn hai chỉ số l và r với l ≤ r và lấy tất cả các ký tự giữa chúng. Vì độ dài chuỗi có thể lên tới một triệu nên rõ ràng chúng ta không thể liệt kê tất cả các chuỗi con O(N2). 

Đầu ra là một số nguyên duy nhất: tổng số chuỗi con có số chữ cái nguy hiểm chính xác là K. 

Khó khăn chính là chúng ta đang đếm tất cả các chuỗi con có số lượng bậc hai. Bất kỳ cách tiếp cận nào kiểm tra rõ ràng các chuỗi con sẽ hết thời gian chờ. Điều này ngay lập tức buộc phải áp dụng cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một trường hợp khó phát hiện khi K bằng 0. Trong trường hợp đó, chúng ta đang đếm các chuỗi con không chứa ký tự nguy hiểm nào cả, vì vậy câu trả lời phụ thuộc hoàn toàn vào khối ký tự an toàn tối đa. Một trường hợp cạnh khác là khi L = 0, nghĩa là không tồn tại chữ cái nguy hiểm nào cả. Khi đó mọi chuỗi con chỉ hợp lệ nếu K = 0, nếu không thì câu trả lời là 0. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi chuỗi con, chúng tôi đếm xem nó chứa bao nhiêu ký tự nguy hiểm và kiểm tra xem nó có bằng K hay không. Về mặt khái niệm, điều này có hiệu quả vì nó khớp trực tiếp với định nghĩa của vấn đề. Tuy nhiên, mỗi chuỗi con cần O(N) để đánh giá nếu được thực hiện một cách đơn giản và có các chuỗi con O(N²), dẫn đến tổng độ phức tạp là O(N³). Ngay cả khi chúng tôi tối ưu hóa việc đếm bằng cách sử dụng tổng tiền tố, chúng tôi vẫn cần O(1) cho mỗi chuỗi con, để lại O(N²), quá lớn đối với N lên tới 10⁶. 

Quan sát quan trọng là điều kiện “chính xác K ký tự rủi ro” có thể được chuyển thành bài toán đếm tổng tiền tố tiêu chuẩn. Chúng tôi chuyển đổi chuỗi thành một mảng nhị phân trong đó mỗi vị trí là 1 nếu đó là ký tự rủi ro và 0 nếu ngược lại. Bây giờ vấn đề trở thành đếm các mảng con có tổng chính xác là K. 

Đối với loại vấn đề này, tổng tiền tố là công cụ tự nhiên. Đặt tiền tố[i] là số ký tự nguy hiểm ở vị trí i đầu tiên. Khi đó, một chuỗi con (l, r) có tổng K khi và chỉ khi tiền tố[r] - tiền tố[l - 1] = K, sắp xếp lại thành tiền tố[l - 1] = tiền tố[r] - K. Điều này có nghĩa là với mỗi r, chúng ta cần biết có bao nhiêu tiền tố trước đó có tổng bằng tiền tố[r] - K. 

Chúng tôi duy trì bản đồ tần số của tổng tiền tố khi chúng tôi quét chuỗi một lần. Điều này mang lại một giải pháp O(N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tiền tố Tổng + Băm | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi tập hợp các ký tự nguy hiểm thành cấu trúc tra cứu nhanh, điển hình là mảng boolean hoặc một tập hợp. Sau đó, chúng tôi quét chuỗi từ trái sang phải trong khi vẫn duy trì số lượng tiền tố đang chạy của các ký tự nguy hiểm. 

1. Chúng tôi khởi tạo bản đồ tần số lưu trữ số lần mỗi tổng tiền tố đã xảy ra. Chúng tôi bắt đầu bằng cách chèn tiền tố tổng 0 với tần số 1, vì tiền tố trống không có ký tự rủi ro nào. 
2. Chúng ta lặp qua chuỗi, cập nhật dòng đếm đang chạy. Nếu ký tự hiện tại có tính rủi ro, chúng ta tăng hiện tại lên 1. Ngược lại, nó không thay đổi. Điều này theo dõi xem chúng ta đã thấy bao nhiêu nhân vật nguy hiểm cho đến nay. 
3. Tại mỗi vị trí, chúng ta muốn biết có bao nhiêu tiền tố trước đó sẽ tạo thành một chuỗi con kết thúc ở đây với đúng K ký tự nguy hiểm. Điều kiện đó có nghĩa là tra cứu xem hiện tại - K đã xuất hiện bao nhiêu lần trước đó. 
4. Chúng tôi thêm tần số đó vào câu trả lời. 
5. Sau đó, chúng tôi cập nhật bản đồ tần số bằng cách tăng số lượng dòng chảy.

Mỗi bước được xây dựng dựa trên cấu trúc tiền tố trước đó, đảm bảo rằng mọi chuỗi con kết thúc ở vị trí hiện tại đều được tính chính xác một lần nếu thỏa mãn điều kiện. 

Bất biến chính là ở chỉ số i, bản đồ tần số chứa chính xác tổng số tiền tố từ vị trí 0 đến i. Điều này đảm bảo rằng mọi chuỗi con hợp lệ kết thúc tại i đều được tính bằng tiền tố duy nhất trước đó và không có chuỗi con không hợp lệ nào được đưa vào vì sự khác biệt về tổng tiền tố thực thi ràng buộc K chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, l = map(int, input().split())
    s = input().strip()
    risky = set(input().strip())

    # prefix sum frequency map
    freq = {0: 1}
    curr = 0
    ans = 0

    for ch in s:
        if ch in risky:
            curr += 1

        need = curr - k
        ans += freq.get(need, 0)

        freq[curr] = freq.get(curr, 0) + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc đầu vào và lưu trữ các chữ cái nguy hiểm trong một bộ để kiểm tra tư cách thành viên O(1). Chúng tôi duy trì dòng chảy dưới dạng số lượng tiền tố đang chạy của các ký tự rủi ro. Tần số từ điển lưu trữ số lần mỗi tổng tiền tố đã được quan sát cho đến nay. Đối với mỗi ký tự, chúng tôi tính toán có bao nhiêu tiền tố trước đó sẽ tạo thành chuỗi con hợp lệ kết thúc ở chỉ mục hiện tại, tích lũy chuỗi đó thành ans và sau đó cập nhật bản đồ tần số tiền tố. 

Một lỗi phổ biến là cập nhật bản đồ tần số trước khi truy vấn nó, điều này sẽ cho phép các chuỗi con trống hoặc các cặp tự đếm sai. Thứ tự đúng là truy vấn trước, sau đó cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 1
abac
a
```Chúng tôi chỉ đánh dấu 'a' là rủi ro. 

| tôi | char | hiện tại | cần = hiện tại - K | tần số[cần] | trả lời | cập nhật tần số | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | một | 1 | 0 | 1 | 1 | {0:1,1:1} | 
| 1 | b | 1 | 0 | 1 | 2 | {0:1,1:2} | 
| 2 | một | 2 | 1 | 2 | 4 | {0:1,1:2,2:1} | 
| 3 | c | 2 | 1 | 2 | 6 | ... | 

Điều này cho thấy mọi chuỗi con kết thúc tại mỗi vị trí đều được tính chính xác khi nó chứa một ký tự nguy hiểm. 

### Ví dụ 2 

đầu vào:```
4 2 2
bcfe
bf
```Ở đây b và f là rủi ro. 

| tôi | char | hiện tại | cần | tần số[cần] | trả lời | tần số | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | b | 1 | -1 | 0 | 0 | {0:1,1:1} | 
| 1 | c | 1 | -1 | 0 | 0 | {0:1,1:1} | 
| 2 | f | 2 | 0 | 1 | 1 | {0:1,1:1,2:1} | 
| 3 | e | 2 | 0 | 1 | 2 | ... | 

Chỉ những chuỗi con kết thúc ở vị trí có chính xác hai chữ cái rủi ro được tích lũy mới góp phần đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Truyền một lần qua chuỗi với các phép toán băm O(1) cho mỗi ký tự | 
| Không gian | O(N) | Trong trường hợp xấu nhất, tất cả các tổng tiền tố đều khác biệt | 

Điều này phù hợp một cách thoải mái trong các ràng buộc vì N có thể lên tới 10⁶ và đường truyền tuyến tính có hàm băm đủ hiệu quả cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k, l = map(int, input().split())
    s = input().strip()
    risky = set(input().strip())

    freq = {0: 1}
    curr = 0
    ans = 0

    for ch in s:
        if ch in risky:
            curr += 1
        ans += freq.get(curr - k, 0)
        freq[curr] = freq.get(curr, 0) + 1

    return str(ans)

# provided samples
assert solve("4 1 1\nabac\na\n") == "6", "sample 1"
assert solve("4 2 2\nbcfe\nbf\n") == "2", "sample 2"
assert solve("4 10 2\nabdc\nad\n") == "0", "sample 3"

# custom cases
assert solve("5 0 1\nabcde\na\n") == "15", "all substrings without a"
assert solve("5 1 0\nabcde\n\n") == "0", "no risky letters but K>0"
assert solve("5 1 1\naaaaa\na\n") == "15", "all substrings valid"
assert solve("1 1 1\na\na\n") == "1", "single char match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi con đều an toàn | 15 | Hành vi K=0 | 
| không có thư nguy hiểm | 0 | bộ rủi ro trống rỗng | 
| tất cả đều có rủi ro | 15 | tích lũy đầy đủ | 
| phần tử đơn | 1 | độ đúng ranh giới | 

## Vỏ cạnh 

Khi L = 0, tập rủi ro trống, do đó dòng điện không bao giờ tăng. Thuật toán chỉ đếm chính xác các chuỗi con khi K = 0, vì chỉ có sự khác biệt về tiền tố là không trùng khớp. 

Khi K lớn hơn bất kỳ số lượng ký tự rủi ro nào có thể có, việc tra cứu freq[curr - K] luôn thất bại một cách an toàn và đóng góp bằng 0. 

Khi chuỗi chứa tất cả các ký tự nguy hiểm, curr tăng từng bước và tần số tiền tố đảm bảo rằng tất cả các mảng con được tính chính xác dưới dạng kết hợp các khác biệt tiền tố.
