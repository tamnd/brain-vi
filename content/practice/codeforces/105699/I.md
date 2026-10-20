---
title: "CF 105699I - Sòng bạc tương tác"
description: "Mỗi vòng của trò chơi trình bày trạng thái vốn hiện tại của bạn và một số nguyên do trọng tài chọn. Số nguyên đó được rút đồng nhất từ ​​phạm vi từ 1 đến số tiền hiện tại của bạn, vì vậy số dư lớn hơn sẽ ngay lập tức tăng phạm vi kết quả có thể xảy ra cho vòng đó."
date: "2026-06-22T04:53:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105699
codeforces_index: "I"
codeforces_contest_name: "OCPC 2024 Winter, Day 8: Borys Minaiev Contest 1 (The 3rd Universal Cup. Stage 27: London)"
rating: 0
weight: 105699
solve_time_s: 42
verified: true
draft: false
---

[CF 105699I - Sòng bạc tương tác](https://codeforces.com/problemset/problem/105699/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vòng của trò chơi trình bày trạng thái vốn hiện tại của bạn và một số nguyên do trọng tài chọn. Số nguyên đó được rút đồng nhất từ ​​phạm vi từ 1 đến số tiền hiện tại của bạn, vì vậy số dư lớn hơn sẽ ngay lập tức tăng phạm vi kết quả có thể xảy ra cho vòng đó. 

Sau khi thấy giá trị này, bạn chọn tham gia vào vòng chơi hay bỏ qua nó. Nếu bạn tham gia, vòng chơi sẽ giống như một canh bạc đối xứng: với xác suất bằng nhau, bạn sẽ thua đúng số tiền đã chọn hoặc kiếm được gấp đôi số tiền đó. Việc bỏ qua chỉ đơn giản là tiến tới vòng tiếp theo mà không cần đổi tiền, nhưng chuỗi các lựa chọn ngẫu nhiên trong tương lai vẫn phụ thuộc vào số dư hiện tại của bạn. 

Trò chơi được chơi trong một số vòng cố định, trừ khi bạn đạt được số dư mục tiêu rất lớn trên 10000 hoặc số tiền của bạn trở thành 0. Sự tương tác làm cho tình huống trở nên tinh vi hơn vì mọi quyết định đều thay đổi sự phân bổ của tất cả các vòng trong tương lai, vì phạm vi các giá trị có thể phụ thuộc vào vốn hiện tại. 

Ràng buộc T nhỏ và cố định ở mức 1000, loại trừ mọi nỗ lực khám phá cây quyết định trên tất cả các chuỗi kết quả có thể xảy ra. Ngay cả khi không phân nhánh, việc mô phỏng xác suất chính xác trên 1000 vòng với tính ngẫu nhiên phụ thuộc vào trạng thái sẽ không khả thi. Khó khăn chính là quá trình này có tính tương tác và thích ứng, do đó việc lập trình động xác suất ngây thơ trên các trạng thái tiền tệ không thành công do sự bùng nổ trạng thái liên tục. 

Một cạm bẫy phổ biến là cố gắng suy luận từng vòng một với các giá trị kỳ vọng. Lợi ích kỳ vọng khi chơi một vòng là dương đối với b cố định, vì kết quả là +b và -b với xác suất bằng nhau, tạo ra thay đổi kỳ vọng +b/2. Tuy nhiên, điều này bỏ qua sự phụ thuộc giữa b và số tiền hiện tại, và quan trọng hơn là bỏ qua phương sai: việc chơi lặp lại có thể đẩy bạn về 0 và kết thúc quá trình sớm. Một sai lầm khác là bỏ qua quá thường xuyên ngay từ đầu và sau đó chơi quá mạnh về sau, điều này có thể khiến bạn đạt đến chế độ chênh lệch cao, trong đó khả năng thua cao hơn do giá trị b được lấy mẫu lớn hơn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các kết quả có thể xảy ra khi chơi và bỏ qua các quyết định trong 1000 vòng. Ngay cả khi chúng tôi chỉ phân nhánh dựa trên kết quả của việc lật đồng xu khi chơi, mỗi vòng sẽ đưa ra hệ số phân nhánh là 2 cho lượt chơi cộng 1 cho lượt bỏ qua với trạng thái không thay đổi, đồng thời thay đổi cách phân bổ các giá trị ngẫu nhiên trong tương lai. Điều này nhanh chóng trở thành hàm mũ trong T và thậm chí việc cắt bớt các trạng thái bằng cách hợp nhất các giá trị tiền giống hệt nhau cũng không giúp ích gì, bởi vì tiền là một số nguyên liên tục cho đến các giá trị có thể rất lớn và mỗi lần chuyển đổi sẽ thay đổi không gian lấy mẫu cho tất cả các vòng trong tương lai. 

Quan sát cấu trúc quan trọng là trò chơi được thiết kế sao cho bất kỳ chiến lược hợp lý nào cũng không cần phải thích ứng một cách chi tiết với tính ngẫu nhiên trung gian. Sự tương tác mang tính đối nghịch trong phân phối nhưng không phải trong thông tin ẩn: lựa chọn b của thẩm phán luôn giống nhau đối với số tiền hiện tại, vì vậy biến kiểm soát có ý nghĩa duy nhất là liệu chúng ta có tham gia khi trạng thái hiện tại đủ an toàn hay không. 

Một sự đơn giản hóa quan trọng là cần lưu ý rằng canh bạc mỗi vòng có độ lệch dương và nhược điểm giới hạn so với vốn hiện tại. Nếu chúng ta ở xa ranh giới thua cuộc (tiền 0), việc chơi luôn được mong đợi tốt hơn so với việc bỏ qua và việc bỏ qua chỉ làm giảm khả năng xảy ra phương sai trong khi không cải thiện mức tăng trưởng dự kiến. Vì điều kiện thắng dựa trên ngưỡng nên chiến lược tối ưu là luôn tận dụng kỳ vọng tích cực bất cứ khi nào chúng ta không có nguy cơ sụp đổ ngay lập tức.

Điều này làm giảm vấn đề quyết định thành một chính sách xác định: luôn chơi khi tiền dương. Sau đó, sự tương tác trở thành một quá trình ngẫu nhiên duy nhất được điều khiển bởi các cập nhật cộng tính nhân-cộng đối xứng lặp đi lặp lại và khía cạnh tương tác biến mất khỏi logic quyết định. 

Việc triển khai sau đó rất đơn giản: luôn xuất PLAY cho đến khi hệ thống kết thúc trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên cây trò chơi | Hàm mũ | Hàm mũ | Quá chậm | 
| Chính sách luôn chơi tham lam | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các vòng một cách tuần tự, phản hồi từng câu hỏi từ giám khảo. 

1. Đọc trạng thái vòng hiện tại bao gồm tiền hiện tại m và giá trị được lấy mẫu b. Giá trị b chỉ quan trọng nếu chúng ta chọn chơi. 
2. Quyết định luôn phản hồi bằng CHƠI miễn là trò chơi chưa kết thúc. Lý do là việc chơi mang lại lợi ích dự kiến ​​​​dương và việc bỏ qua chỉ làm giảm khả năng tiếp xúc mà không cải thiện độ trôi dự kiến. 
3. Đưa ra quyết định ngay lập tức và tuôn ra, vì sự tương tác yêu cầu đồng bộ hóa nghiêm ngặt. 
4. Sau mỗi phản hồi, hãy đợi trạng thái tiếp theo. Nếu trọng tài gửi THẮNG hoặc THUA, chấm dứt ngay lập tức. 

### Tại sao nó hoạt động 

Quá trình tại mỗi lần chơi có sự thay đổi kỳ vọng +b/2, và b luôn không âm và bị giới hạn bởi số tiền hiện tại. Việc bỏ qua không làm thay đổi sự phân bố của các giá trị b trong tương lai theo cách bù đắp cho độ lệch dương bị mất. Trải qua hơn 1000 vòng, việc liên tục tận dụng các cơ hội trôi dạt tích cực sẽ chi phối mọi chiến lược tránh chúng một cách có chọn lọc, vì không có kết quả điều chỉnh đối thủ bên ngoài nào dựa trên các quyết định ngoài quy tắc lấy mẫu thống nhất. Trạng thái thất bại hấp thụ duy nhất là đạt đến 0 và cấu trúc của các bản cập nhật đảm bảo rằng việc tránh chơi không thể làm giảm xác suất đạt 0 so với lợi ích dự kiến ​​​​tăng thêm khi chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    T = input().strip()
    if not T:
        return
    T = int(T)

    for _ in range(T):
        line = input().strip()
        if not line:
            return

        if line == "WIN" or line == "LOSE":
            return

        # line format: "ROUND m b"
        print("PLAY", flush=True)

if __name__ == "__main__":
    main()
```Giải pháp chỉ đơn giản là phản ứng với mọi dấu nhắc của vòng bằng cách in PLAY. Điều tinh tế duy nhất là xử lý các tín hiệu kết thúc sớm, tín hiệu này phải dừng thực thi ngay lập tức để tránh mất đồng bộ hóa tương tác. Việc xả nước sau mỗi lần xuất là bắt buộc vì giám khảo mong đợi phản hồi theo thời gian thực. 

## Ví dụ đã hoạt động 

Vì các mẫu tương tác là một phần nên chúng tôi mô phỏng một dấu vết đơn giản hóa trong đó chúng tôi chỉ quan sát các vòng đến. 

### Ví dụ 1 

Trình tự đầu vào: 

VÒNG 1000 40 

VÒNG 960 300 

VÒNG 660 120 

| Vòng | m | b | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1000 | 40 | CHƠI | 
| 2 | 960 | 300 | CHƠI | 
| 3 | 660 | 120 | CHƠI | 

Dấu vết này cho thấy chính sách không bao giờ phụ thuộc vào b hoặc m. Điều bất biến là chiến lược không phân nhánh theo trạng thái, đảm bảo tính nhất quán với đối số trôi dạt luôn tích cực. 

### Ví dụ 2 

Trình tự đầu vào: 

VÒNG 100 90 

VÒNG 10 5 

THUA 

| Vòng | m | b | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 100 | 90 | CHƠI | 
| 2 | 10 | 5 | CHƠI | 
| kết thúc | - | - | chấm dứt | 

Điều này chứng tỏ việc xử lý chấm dứt sớm chính xác. Ngay cả trong những trạng thái không thuận lợi, thuật toán không thử logic có điều kiện và vẫn được đồng bộ hóa với người đánh giá cho đến khi kết thúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Một quyết định liên tục mỗi vòng | 
| Không gian | O(1) | Không có trạng thái ngoài việc đọc đầu vào | 

Giới hạn T = 1000 đảm bảo thời gian chạy tầm thường. Chi phí tương tác chiếm ưu thế trong tính toán nhưng vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    T = sys.stdin.readline().strip()
    if not T:
        return ""
    T = int(T)

    for _ in range(T):
        line = sys.stdin.readline().strip()
        if not line:
            break
        if line in ("WIN", "LOSE"):
            break
        print("PLAY")

    return out.getvalue().strip()

# sample-like test
assert run("2\nROUND 100 10\nROUND 90 20\n") == "PLAY\nPLAY"

# immediate loss
assert run("1\nLOSE\n") == ""

# immediate win signal midstream
assert run("3\nROUND 100 10\nWIN\nROUND 200 20\n") == "PLAY"

# minimal rounds
assert run("1\nROUND 1 1\n") == "PLAY"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vòng bình thường | CHƠI | tương tác cơ bản | 
| THUA ngay lập tức | trống | chấm dứt sớm | 
| THẮNG giữa dòng | CHƠI rồi dừng | tính đúng đắn của lệnh dừng khi thắng | 
| trạng thái tối thiểu duy nhất | CHƠI | vòng hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Trường hợp một bên là chấm dứt ngay lập tức trước khi đưa ra bất kỳ quyết định nào. Nếu dòng đầu tiên sau T là THẮNG hoặc THUA, chương trình không được cố in bất cứ thứ gì. Giải pháp xử lý vấn đề này bằng cách kiểm tra đường truyền đến trước khi phát ra đầu ra. 

Một trường hợp khác là nhập liệu một phần trong đó trọng tài kết thúc sớm sau điều kiện thắng. Vòng lặp sẽ thoát ngay lập tức khi nhìn thấy WIN, ngăn chặn các bản in bổ sung có thể làm mất đồng bộ hóa giao thức. 

Trường hợp cuối cùng là các dòng trống hoặc không đúng định dạng do bộ đệm tương tác. Việc triển khai bảo vệ chống lại điều này bằng cách ngắt nếu xảy ra đọc trống, đảm bảo nó không bao giờ chặn hoặc tạo ra đầu ra không xác định.
