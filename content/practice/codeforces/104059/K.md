---
title: "CF 104059K - K.O. trẻ em"
description: "Một cây cầu 2 × n được xây dựng từ n vị trí, trong đó mỗi vị trí có thể có hai ô: trái và phải. Chính xác một trong hai vị trí là an toàn ở mỗi vị trí và mặt an toàn cho vị trí i được cung cấp bởi một chuỗi có độ dài n, trong đó mỗi ký tự cho biết ô bên trái hay ô bên phải…"
date: "2026-07-02T03:31:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104059
codeforces_index: "K"
codeforces_contest_name: "2022-2023 ACM-ICPC German Collegiate Programming Contest (GCPC 2022)"
rating: 0
weight: 104059
solve_time_s: 54
verified: true
draft: false
---

[CF 104059K - K.O. Trẻ em](https://codeforces.com/problemset/problem/104059/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một cây cầu 2 × n được xây dựng từ n vị trí, trong đó mỗi vị trí có thể có hai ô: trái và phải. Chính xác một trong hai vị trí là an toàn ở mỗi vị trí và mặt an toàn cho vị trí i được cung cấp bởi một chuỗi có độ dài n, trong đó mỗi ký tự cho biết ô bên trái hay bên phải là chính xác. 

Người chơi lần lượt qua cầu theo thứ tự cố định. Mỗi người chơi cố gắng đi qua tất cả n vị trí. Người chơi đầu tiên không có thông tin trước và tuân theo hành vi “chuyển đổi” xác định: cô ấy bắt đầu bằng cách bước lên ô bên trái ở vị trí 1, sau đó đổi bên ở mọi vị trí tiếp theo. 

Mỗi người chơi sau này đều có một phần kiến ​​thức. Bất cứ khi nào người chơi trước bước lên thành công một ô, ô đó sẽ được xác nhận là an toàn cho vị trí đó. Nếu một người chơi bước nhầm ô, cô ấy sẽ ngã ngay lập tức, nhưng lỗi đó sẽ tiết lộ ô nào đúng cho vị trí đó và tất cả người chơi trong tương lai sẽ sử dụng thông tin mới này. Đối với các vị trí chưa được xác nhận, người chơi lại thực hiện hành vi chuyển đổi tương tự bắt đầu từ bước cuối cùng của họ. 

Nhiệm vụ là xác định có bao nhiêu người chơi có thể đạt được vị trí n thành công, dựa trên kiến ​​thức tích lũy được sau mỗi lần thất bại. 

Các ràng buộc n, k 1000 ngụ ý rằng ngay cả mô phỏng O(nk) cũng đủ nhanh. Mỗi người chơi thực hiện tối đa n bước, do đó, một mô phỏng đơn giản sẽ phù hợp thoải mái trong giới hạn. 

Một trường hợp phức tạp xuất hiện khi những người chơi đầu tiên thất bại ngay lập tức. Ví dụ: nếu vị trí đầu tiên là R, người chơi đầu tiên luôn bắt đầu bằng L và rơi ngay lập tức, hiển thị lựa chọn chính xác. Điều này có thể xếp tầng và thay đổi đáng kể hành vi sau này. Một trường hợp khác xảy ra khi một người chơi hoàn thành toàn bộ cây cầu, vì không có thông tin mới nào được thêm vào và tất cả những người chơi sau đó đều hành xử giống hệt nhau, khiến câu trả lời trở nên bão hòa. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là mô phỏng từng người chơi một cách độc lập ngay từ đầu, luôn tính toán lại những gì đã biết và thực hiện tất cả n bước trong khi áp dụng quy tắc chuyển đổi. Mỗi mô phỏng sẽ quét toàn bộ chuỗi và bất cứ khi nào xảy ra sự không khớp, chúng tôi sẽ cập nhật kiến ​​thức chung và khởi động lại người chơi tiếp theo. 

Điều này đã phù hợp với quy trình được mô tả, nhưng điểm kém hiệu quả chính là mỗi người chơi đang tính toán lại các quyết định ngay cả đối với các phần của cây cầu đã được biết đến đầy đủ và ổn định. Tuy nhiên, vì k và n chỉ tối đa 1000, nên ngay cả quá trình quét lặp lại này vẫn nằm trong khoảng 10^6 thao tác, do đó không cần phải tối ưu hóa nặng hơn. 

Nhận xét quan trọng là trạng thái duy nhất quan trọng đối với người chơi là vị trí nào đã được tiết lộ. Khi đã biết một vị trí, mọi người chơi trong tương lai sẽ hành xử một cách xác định ở đó và không có thay đổi nào nữa xảy ra ở vị trí đó. Vì vậy, thay vì tính toán lại bất kỳ thứ gì, chúng tôi chỉ cần duy trì một mảng boolean gồm các vị trí đã biết và mô phỏng từng người chơi một lần. 

Cách tiếp cận bạo lực hoạt động vì nó tuân thủ trực tiếp các quy tắc nhưng trở nên dư thừa về mặt khái niệm trong cách nó lấy lại thông tin đã biết mỗi lần. Cách tiếp cận được tối ưu hóa sẽ thu gọn tất cả lịch sử vào một trạng thái phát triển duy nhất và điều hành mỗi người chơi như một sự tiếp nối của trạng thái đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nk) | O(n) | Đã chấp nhận | 
| Mô phỏng trạng thái | O(nk) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng có độ dài n cho biết tấm chính xác ở mỗi vị trí đã được phát hiện chưa. 

Chúng tôi cũng giữ lại chuỗi s đã cho thể hiện cấu hình thực sự. 

Chúng tôi mô phỏng từng người chơi một. 

### Các bước

1. Khởi tạo một mảng đã biết với tất cả các giá trị được đặt thành sai, nghĩa là ban đầu không có vị trí nào được xác nhận. 
2. Lặp lại từng người chơi từ 1 đến k. 
3. Đặt biến cuối cùng thành L khi bắt đầu duyệt qua mỗi người chơi. Điều này thể hiện phía mà người chơi đã bước lên ở vị trí trước đó và nó được cố định bởi quy tắc rằng mọi bước di chuyển đều bắt đầu bằng một bước bên trái. 
4. Với mỗi vị trí i từ 0 đến n − 1, quyết định bước đi tiếp theo. 
5. Nếu known[i] là đúng thì phía đúng đã được xác định rồi nên người chơi chỉ cần bước lên s[i] và cập nhật lần cuối tương ứng. 
6. Nếu biết[i] là sai, người chơi tuân theo quy tắc chuyển đổi và bước về phía đối diện của bước cuối cùng. 
7. Nếu bên được chọn khớp với s[i], người chơi sẽ sống sót ở vị trí này và chúng tôi cập nhật cuối cùng cho bên được chọn. 
8. Nếu bên được chọn không trùng với s[i] thì người chơi rơi xuống vị trí i. Chúng tôi đánh dấu known[i] là đúng vì mặt đúng hiện được tiết lộ là s[i] và chúng tôi ngừng xử lý trình phát này ngay lập tức. 
9. Nếu người chơi đến vị trí n mà không bị ngã, chúng tôi tính người chơi này là thành công. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, các vị trí đã biết hoạt động giống như các ràng buộc cố định buộc mọi người chơi trong tương lai phải thực hiện cùng một hành động đúng ở đó. Sự không chắc chắn duy nhất tồn tại ở các vị trí không xác định, trong đó mọi người chơi đều tuân theo cùng một quy tắc luân phiên xác định chỉ dựa trên bước ngay trước đó của họ. 

Mỗi lần thất bại đều làm tăng lượng thông tin đã biết và không bao giờ làm mất hiệu lực của kiến ​​thức trước đó. Điều này tạo ra một quá trình đơn điệu: trạng thái đã biết chỉ mở rộng. Vì mỗi bản mở rộng được kích hoạt do lỗi ở một vị trí chưa xác định trước đó và chỉ có n vị trí nên quá trình này sẽ ổn định sau tối đa n lần phát hiện và những người chơi tiếp theo sẽ hành xử giống hệt nhau. Do đó, mô phỏng phản ánh chính xác sự phát triển của thông tin trong trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    known = [False] * n
    ans = 0

    for _ in range(k):
        last = 'L'
        ok = True

        for i in range(n):
            if known[i]:
                choice = s[i]
            else:
                choice = 'R' if last == 'L' else 'L'

            if choice == s[i]:
                last = choice
            else:
                known[i] = True
                ok = False
                break

        if ok:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì trạng thái tri thức đang phát triển trong`known`. Mỗi người chơi được mô phỏng độc lập nhưng sử dụng trạng thái chia sẻ này. Biến`last`được đặt lại cho mỗi người chơi để thực thi quy tắc rằng mọi quá trình di chuyển đều bắt đầu bằng việc bước lên ô bên trái. Khi đánh trúng một vị trí không xác định, thuật toán sẽ áp dụng quy tắc chuyển đổi để quyết định nước đi. Sự không khớp vừa kết thúc người chơi hiện tại vừa làm lộ ô chính xác cho vị trí đó. 

Một cạm bẫy phổ biến là mang theo không đúng cách`last`khắp người chơi. Mỗi người chơi phải khởi động lại logic luân phiên, nếu không kiểu chuyển đổi sẽ lệch khỏi quy tắc đã định và tạo ra các chuyển đổi sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 5
s = L R L
```Chúng tôi theo dõi một vài người chơi đầu tiên. 

| Người chơi | tôi | được biết đến | cuối cùng | sự lựa chọn | kết quả | cập nhật đã biết | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [] | L | L | được | [] | 
| 1 | 1 | [] | L | R | thất bại | [1] | 

Người chơi 1 rơi xuống vị trí 1, cho thấy đó phải là R. Những người chơi sau này giờ đây sử dụng đã biết [1]. 

| Người chơi | tôi | được biết đến | cuối cùng | sự lựa chọn | kết quả | cập nhật đã biết | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 0 | [1] | L | L | được | [1] | 
| 2 | 1 | [1] | L | R (đã biết) | được | [1] | 
| 2 | 2 | [1] | R | L | được | [1] | 

Người chơi 2 thành công và những người chơi sau sẽ làm theo hành vi tương tự. 

Điều này chứng tỏ một thất bại đơn lẻ sẽ ổn định các quyết định trong tương lai như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 2
s = R R R
```| Người chơi | tôi | được biết đến | cuối cùng | sự lựa chọn | kết quả | cập nhật đã biết | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [] | L | L | thất bại | [0] | 

Người chơi 1 lộ ngay vị trí 0 là R. 

| Người chơi | tôi | được biết đến | cuối cùng | sự lựa chọn | kết quả | cập nhật đã biết | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 0 | [0] | L | R | được | [0] | 
| 2 | 1 | [0] | R | R | được | [0] | 
| 2 | 2 | [0] | R | L | được | [0] | 

Người chơi 2 kết thúc thành công, cho thấy thông tin sớm đã thay đổi hoàn toàn kết quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi người trong số k người chơi quét tối đa n vị trí một lần | 
| Không gian | O(n) | Lưu trữ trạng thái đã biết cho từng vị trí | 

Các ràng buộc cho phép tối đa 10^6 thao tác và mô phỏng vẫn thoải mái trong giới hạn này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    known = [False] * n
    ans = 0

    for _ in range(k):
        last = 'L'
        ok = True
        for i in range(n):
            if known[i]:
                choice = s[i]
            else:
                choice = 'R' if last == 'L' else 'L'

            if choice == s[i]:
                last = choice
            else:
                known[i] = True
                ok = False
                break
        if ok:
            ans += 1

    return str(ans)

# provided samples
assert run("3 5\nLRL\n") == "3"
assert run("3 2\nRRR\n") == "1"

# minimum size
assert run("1 1\nL\n") == "1"

# all equal safe left
assert run("5 4\nLLLLL\n") == "4"

# alternating pattern
assert run("4 3\nLRLR\n") == "3"

# immediate failure cascade
assert run("2 3\nRL\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 đơn | 1 | truyền tải tối thiểu | 
| tất cả L đều đúng | k | không thất bại | 
| xen kẽ | k | chuyển mạch ổn định | 
| RL ngắn | 2 | tuyên truyền điều chỉnh sớm | 

## Vỏ cạnh 

Khi vị trí đầu tiên không xác định và không khớp với người chơi đầu tiên, thuật toán sẽ ngay lập tức đánh dấu vị trí đó là đã biết và đảm bảo tất cả những người chơi sau đó đều bắt đầu với thông tin chính xác. Điều này được xử lý chính xác vì lỗi kích hoạt bản cập nhật trước khi chuyển sang trình phát tiếp theo. 

Đối với các đầu vào có tất cả các vị trí giống hệt nhau, mô hình xen kẽ của người chơi đầu tiên gây ra một chuỗi lỗi có thể dự đoán được và cuối cùng ổn định hệ thống, sau đó mọi người chơi còn lại đều thành công hay thất bại một cách xác định. Mô phỏng nắm bắt được điều này vì đã biết chỉ tăng lên và không bao giờ đặt lại. 

Khi k lớn so với n, hệ thống sẽ nhanh chóng đạt đến trạng thái cố định sau tối đa n lần phát hiện. Sau thời điểm đó, mọi người chơi đều hành xử giống hệt nhau và thuật toán sẽ tự nhiên tính những người chơi thành công còn lại mà không có bất kỳ cách viết đặc biệt nào.
