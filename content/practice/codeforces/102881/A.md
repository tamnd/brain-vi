---
title: "CF 102881A - Sĩ quan Anany Thu thập chuỗi chuỗi"
description: "Chúng ta cần tìm phần liền kề ngắn nhất của chuỗi có thể đi từ trái sang phải và dùng để tập hợp các chữ cái A, B, C, ..., Z theo thứ tự. Phần được chọn không cần phải trực tiếp bằng bảng chữ cái."
date: "2026-07-25T12:33:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "A"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 37
verified: true
draft: false
---

[CF 102881A - Sĩ quan Anany Thu thập chuỗi con](https://codeforces.com/problemset/problem/102881/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tìm phần liền kề ngắn nhất của chuỗi có thể đi từ trái sang phải và được sử dụng để thu thập các chữ cái`A`,`B`,`C`, ...,`Z`theo thứ tự. Phần được chọn không cần phải trực tiếp bằng bảng chữ cái. Cho phép thêm các ký tự, miễn là tồn tại một dãy con bên trong nó chính xác là bảng chữ cái theo thứ tự tăng dần. 

Mỗi trường hợp thử nghiệm đưa ra một chuỗi các chữ cái tiếng Anh viết hoa. Câu trả lời là độ dài tối thiểu của chuỗi con có các ký tự chứa bảng chữ cái đầy đủ dưới dạng một chuỗi con. Đầu vào đảm bảo rằng chuỗi con như vậy tồn tại. 

Độ dài của chuỗi nhiều nhất là 777 và số lượng ca kiểm thử nhiều nhất là 44. Các giới hạn này đủ nhỏ để có thể thực hiện được các giải pháp bậc hai. Một giải pháp thử từng chuỗi con và kiểm tra cẩn thận có thể chấp nhận được nếu việc kiểm tra hiệu quả. Cách tiếp cận hình khối vẫn có rủi ro vì 777³ có gần nửa tỷ phép tính trong trường hợp xấu nhất. 

Các trường hợp cạnh chính là do các bản sao bổ sung của các chữ cái và bởi các chuỗi con chứa mọi ký tự nhưng không theo thứ tự bắt buộc. 

Ví dụ, hãy xem xét:```
1
26
ABCDEFGHIJKLMNOPQRSTUVWXYZ
```Câu trả lời là:```
26
```Việc triển khai bất cẩn chỉ tính xem mọi chữ cái xuất hiện có hiệu quả ở đây hay không, nhưng ý tưởng đó không thành công đối với các chuỗi có thứ tự sai. 

Ví dụ:```
1
26
ZYXWVUTSRQPONMLKJIHGFEDCBA
```Câu trả lời không hợp lệ từ dữ liệu đầu vào này vì không thể hình thành chuỗi bảng chữ cái. Cách tiếp cận dựa trên tần số sẽ nghĩ sai rằng mọi ký tự bắt buộc đều tồn tại. 

Một trường hợp phức tạp khác là khi một cửa sổ ngắn hơn bị ẩn bên trong một vùng hợp lệ lớn hơn. 

Ví dụ:```
1
29
XXXABCDEFGHIJKLMNOPQRSTUVWXYZ
```Câu trả lời là:```
26
```Giải pháp tìm thấy chuỗi con hợp lệ đầu tiên và không bao giờ thu nhỏ chuỗi con đó sẽ tạo ra 29, thiếu câu trả lời tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi chuỗi con có thể. Đối với mỗi vị trí bắt đầu, chúng tôi mở rộng vị trí kết thúc từng ký tự một và kiểm tra xem chuỗi con hiện tại có chứa bảng chữ cái dưới dạng chuỗi con hay không. Nếu đúng như vậy, chúng tôi ghi lại độ dài của nó và tiếp tục tìm kiếm độ dài nhỏ hơn. 

Phương pháp này đúng vì mọi câu trả lời có thể có đều là một chuỗi con nào đó và chúng tôi kiểm tra tất cả chúng một cách rõ ràng. Phần đắt tiền là quá trình kiểm tra. Có các chuỗi con O(n²) và nếu mỗi lần kiểm tra quét chuỗi con đó thì trường hợp xấu nhất sẽ trở thành O(n³). Với n = 777, đây là khoảng 470 triệu lượt kiểm tra ký tự, điều này là không cần thiết. 

Quan sát quan trọng là tính chất chúng ta cần là tính đơn điệu. Nếu một chuỗi con chứa`ABCDEFGHIJKLMNOPQRSTUVWXYZ`dưới dạng một chuỗi con, việc thêm nhiều ký tự hơn không thể làm cho thuộc tính đó sai. Cửa sổ hợp lệ vẫn hợp lệ khi được mở rộng. Điều này cho phép chúng ta sử dụng một cửa sổ trượt. 

Chúng tôi giữ hai con trỏ mô tả một cửa sổ hiện tại. Chúng tôi mở rộng phía bên phải cho đến khi cửa sổ trở nên hợp lệ. Khi nó hợp lệ, việc xóa các ký tự ở bên trái chỉ có thể làm cho nó ngắn hơn, vì vậy chúng tôi liên tục thu nhỏ từ bên trái trong khi thuộc tính vẫn đúng. Điều này tìm thấy cửa sổ hợp lệ ngắn nhất kết thúc ở con trỏ bên phải hiện tại. 

Câu hỏi còn lại là làm thế nào để kiểm tra tính hợp lệ một cách nhanh chóng. Vì dãy con bắt buộc chỉ có 26 ký tự nên chúng ta có thể lưu trữ thông tin mới nhất cần thiết để so khớp. Đối với cửa sổ hiện tại, chúng tôi quét nó và khớp một cách tham lam với chữ cái được yêu cầu tiếp theo. Kích thước cửa sổ tối đa là 777, vì vậy séc này rẻ. Một cách triển khai khác có thể duy trì nhiều trạng thái hơn, nhưng phiên bản đơn giản đã đủ nhanh. 

Phương pháp vũ phu có hiệu quả vì nó kiểm tra mọi ứng viên. Nó thất bại vì nó lặp lại công việc tương tự trên các chuỗi con chồng chéo. Cửa sổ trượt tránh sự lặp lại này bằng cách sử dụng thực tế là cửa sổ hợp lệ có thể được rút ngắn một cách an toàn từ bên trái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Cửa Sổ Trượt | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một cửa sổ trống và đặt cả hai con trỏ vào đầu chuỗi. Cửa sổ hiện tại đại diện cho phần chuỗi chúng ta đang kiểm tra. 
2. Di chuyển con trỏ phải về phía trước và thêm các ký tự mới vào cửa sổ. Sau mỗi lần mở rộng, hãy kiểm tra xem cửa sổ hiện tại có chứa bảng chữ cái dưới dạng dãy con hay không. Việc mở rộng là cần thiết vì cửa sổ ngắn hơn không thể xuất hiện trước khi chúng tôi tìm thấy cửa sổ hợp lệ. 
3. Khi cửa sổ hợp lệ, hãy cập nhật câu trả lời với độ dài hiện tại của nó. Điều này ghi lại giải pháp tốt nhất kết thúc ở con trỏ bên phải này. 
4. Di chuyển con trỏ trái về phía trước trong khi cửa sổ vẫn hợp lệ. Mỗi lần xóa thành công sẽ tạo ra một chuỗi con hợp lệ nhỏ hơn, vì vậy nó cũng phải được xem xét. 
5. Tiếp tục cho đến khi con trỏ bên phải chạm đến cuối chuỗi. Cửa sổ ghi nhỏ nhất chính là câu trả lời. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến rằng mỗi khi cửa sổ hợp lệ thì đó là cửa sổ hợp lệ nhỏ nhất kết thúc ở ranh giới bên phải hiện tại của nó sau giai đoạn thu nhỏ. Bất kỳ câu trả lời tối ưu nào cũng có một số ranh giới đúng. Khi thuật toán đạt đến ranh giới đó, nó sẽ mở rộng đủ để trở thành hợp lệ và sau đó loại bỏ mọi ký tự không cần thiết ở bên trái. Bởi vì tất cả các ranh giới quyền có thể có đều được xem xét nên mức tối thiểu toàn cục được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid(s, l, r):
    need = 0
    for i in range(l, r + 1):
        if s[i] == chr(ord('A') + need):
            need += 1
            if need == 26:
                return True
    return False

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        left = 0
        best = n

        for right in range(n):
            while left <= right and valid(s, left, right):
                best = min(best, right - left + 1)
                left += 1

        ans.append(str(best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`valid`Hàm kiểm tra xem chuỗi con hiện tại có thể tạo ra bảng chữ cái hay không. Nó tham lam tìm kiếm chữ cái được yêu cầu tiếp theo. Khi tìm thấy tất cả 26 chữ cái, hàm sẽ trả về ngay lập tức vì không thể quét thêm nữa có thể thay đổi kết quả. 

Vòng lặp chính điều khiển cửa sổ trượt. Con trỏ bên phải luôn di chuyển về phía trước, thêm nhiều ký tự hơn. Con trỏ trái chỉ di chuyển về phía trước sau khi cửa sổ trở nên hợp lệ, do đó mỗi vị trí được xử lý theo thứ tự và không bao giờ di chuyển lùi. 

Điều kiện biên trong vòng lặp thu hẹp là quan trọng. Chúng ta phải cho phép kiểm tra cửa sổ một ký tự, vì vậy điều kiện là`left <= right`. Câu trả lời được khởi tạo thành`n`bởi vì toàn bộ chuỗi luôn là giới hạn trên hợp lệ theo bảo đảm sự cố. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
35
FORCESABCDEFDIVGHIJKLMNOPQRSTUVWXYZ
```Những thay đổi trạng thái quan trọng là: 

| Trái | Đã thêm ký tự bên phải | Kết quả hiện tại | 
| --- | --- | --- | 
| 0 | F | Không hợp lệ | 
| 1 | Ồ | Không hợp lệ | 
| 6 | A | Bắt đầu khớp bảng chữ cái | 
| 34 | Z | Cửa sổ trở nên hợp lệ | 
| 13 | Z | Cửa sổ hợp lệ nhỏ nhất được tìm thấy | 

Chuỗi con hợp lệ bắt đầu ở đầu tiên`A`cho phép chuỗi bảng chữ cái tiếp tục. Các ký tự trước thời điểm đó là không cần thiết nên việc thu nhỏ sẽ loại bỏ chúng. 

Một ví dụ khác:```
1
29
XXXABCDEFGHIJKLMNOPQRSTUVWXYZ
```| Trái | Đúng | Chiều dài cửa sổ | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 25 | 26 | Có | 
| 1 | 25 | 25 | Không | 
| 0 | 28 | 29 | Có | 

Đầu tiên, thuật toán tìm toàn bộ cửa sổ, sau đó loại bỏ các ký tự đầu không cần thiết. Câu trả lời cuối cùng là 26, cho thấy tại sao cần phải thu nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Cửa sổ có thể được kiểm tra O(n) lần qua O(n) chuyển động của con trỏ. | 
| Không gian | O(1) | Chỉ có con trỏ và bộ đếm được lưu trữ. | 

Với n giới hạn ở 777, nghiệm bậc hai dễ dàng nằm trong giới hạn đó. Hệ số không đổi nhỏ vì mỗi lần kiểm tra tính hợp lệ chỉ tìm kiếm 26 chữ cái có thứ tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def valid(s, l, r):
        need = 0
        for i in range(l, r + 1):
            if s[i] == chr(ord('A') + need):
                need += 1
                if need == 26:
                    return True
        return False

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        left = 0
        best = n
        for right in range(n):
            while left <= right and valid(s, left, right):
                best = min(best, right - left + 1)
                left += 1
        res.append(str(best))

    sys.stdin = old
    return "\n".join(res)

assert run("""3
35
FORCESABCDEFDIVGHIJKLMNOPQRSTUVWXYZ
34
ABCDEFDIVGHIJKLMNICPCOPQRSTUVWXYZO
39
AINBSHAMSCPCDEFGHIJKLMANANYOPQRSTUVWXYZ
""") == """29
33
39"""

assert run("""1
26
ABCDEFGHIJKLMNOPQRSTUVWXYZ
""") == "26"

assert run("""1
29
XXXABCDEFGHIJKLMNOPQRSTUVWXYZ
""") == "26"

assert run("""1
52
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
""") == "26"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thứ tự bảng chữ cái | 26 | Câu trả lời tối thiểu có thể | 
| Ký tự phụ trước bảng chữ cái | 26 | Thu nhỏ đúng bên trái | 
| Hai bảng chữ cái liên tiếp | 26 | Tìm cửa sổ tối ưu đầu tiên | 
| Mẫu chính thức | 29, 33, 39 | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với bảng chữ cái đảo ngược, thuật toán không bao giờ đánh dấu cửa sổ là hợp lệ vì thứ tự khớp tham lam không thể tiến lên từ`A`ĐẾN`B`và trở đi. Giải pháp chỉ có tần số sẽ thất bại ở đây vì nó bỏ qua thứ tự. 

Đối với một chuỗi có các ký tự không cần thiết trước câu trả lời, thuật toán sẽ mở rộng cho đến khi tìm thấy bảng chữ cái và sau đó ngay lập tức kiểm tra việc loại bỏ các ký tự đó. Đây chính xác là lý do tại sao cửa sổ trượt không dừng ở chuỗi con hợp lệ đầu tiên. 

Đối với một chuỗi chứa nhiều dãy chữ cái có thể có, việc kiểm tra tính hợp lệ tham lam là đủ vì chúng ta chỉ cần sự tồn tại. Sự xuất hiện sớm nhất có thể của mỗi chữ cái bắt buộc tiếp theo luôn để lại khoảng trống lớn nhất cho các chữ cái sau, vì vậy việc tìm ra một dãy con hoàn chỉnh chứng tỏ rằng cửa sổ là hợp lệ. 

Bạn có thể điều chỉnh phong cách biên tập này cho phù hợp với các vấn đề về cửa sổ tiếp theo khác bằng cách thay thế kiểm tra tính hợp lệ trong khi vẫn giữ nguyên lý luận về cửa sổ thu nhỏ tương tự.
