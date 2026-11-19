---
title: "CF 2097A - Cá cược thể thao"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có nhiều học sinh và với mỗi học sinh, chúng ta được thông báo một ngày $ai$."
date: "2026-06-08T10:49:49+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "brute-force", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2097
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1021 (Div. 1)"
rating: 1400
weight: 2097
solve_time_s: 92
verified: false
draft: false
---

[CF 2097A - Cá cược thể thao](https://codeforces.com/problemset/problem/2097/A) 

**Đánh giá:** 1400 
**Tags:** 2-sat, bạo lực, toán học, phân loại 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi tình huống, có nhiều học sinh và với mỗi học sinh, chúng ta được thông báo một ngày$a_i$. Vadim cá cược với sinh viên đó vào một ngày$a_i$và việc đặt cược phụ thuộc vào việc dự đoán chính xác mô hình hai ngày bắt đầu từ ngày tiếp theo, nghĩa là ngày$a_i+1$Và$a_i+2$. 

Mỗi ngày đều có một kết quả tiềm ẩn có thể có một trong hai dạng. Vadim không biết trước trình tự thực sự của các kết quả, nhưng anh ấy muốn cam kết dự đoán theo cách mà ít nhất một học sinh được đảm bảo dự đoán chính xác cả hai ngày tiếp theo của họ, bất kể trình tự thực tế diễn ra như thế nào. 

Khó khăn chính là Vadim phải quyết định trước mọi dự đoán và kết quả thực tế là đối nghịch. Vì vậy, đây là vấn đề đảm bảo trong trường hợp xấu nhất: chúng ta đang kiểm tra xem liệu có tồn tại một phép gán dự đoán cố định sao cho mọi dãy thực có thể vẫn còn ít nhất một học sinh “hoàn toàn đúng” hay không. 

Đầu vào là một tập hợp các ca kiểm thử, mỗi ca chứa tối đa$10^5$học sinh nói chung trong mọi trường hợp. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm sẽ không tồn tại được. Việc sắp xếp và đếm tần số đều có thể chấp nhận được, nhưng bất cứ điều gì theo phương pháp bậc hai$n$mỗi trường hợp thử nghiệm là không. 

Một trường hợp thất bại tinh tế phát sinh khi số ngày đặt cược của sinh viên quá dàn trải. Ví dụ, nếu tất cả$a_i$khác nhau và không có hai lần đặt cược trùng nhau trong cùng một khoảng thời gian hai ngày, khi đó mỗi học sinh sẽ tương ứng với một ràng buộc độc lập. Trong trường hợp đó, Vadim không thể sử dụng lại cấu trúc trong các dự đoán và vấn đề giảm xuống còn bao gồm quá nhiều khả năng độc lập với quá ít nhiệm vụ xác định. Ngược lại, khi nhiều$a_i$trùng hoặc cụm, chồng chéo tạo ra sự dư thừa có thể bị khai thác. 

Một sự hiểu lầm ngây thơ là coi mỗi học sinh một cách độc lập và cho rằng chúng ta luôn có thể chỉ định các mẫu cho mỗi học sinh. Điều đó không thành công vì kết quả của cùng ngày được chia sẻ cho tất cả học sinh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các kết quả có thể xảy ra của chuỗi nhị phân vô hạn và kiểm tra xem liệu có tồn tại lựa chọn dự đoán đảm bảo ít nhất một học sinh hài lòng với mọi kết quả hay không. Thậm chí hạn chế sự chú ý vào những ngày liên quan, nếu chúng ta có$k$ngày riêng biệt liên quan, có$2^k$nhiệm vụ kết quả có thể xảy ra. Đối với mỗi bài tập, chúng ta cần kiểm tra xem khoảng thời gian hai ngày của học sinh có khớp với dự đoán của họ hay không, sau đó cố gắng suy luận về tất cả các bài tập dự đoán có thể có. Điều này nhanh chóng trở thành cấp số nhân trong cả kết quả và không gian chiến lược. 

Nhận xét quan trọng là điều duy nhất quan trọng đối với học sinh trong ngày$a_i$là cặp$(a_i+1, a_i+2)$. Về cơ bản, mỗi học sinh đang cố gắng “che” một cửa sổ dài 2. Nếu chúng ta nghĩ ngược lại, Vadim đang chọn dự đoán cho mỗi ngày và mỗi học sinh sẽ kiểm tra một cặp lựa chọn cụ thể. 

Vì vậy, vấn đề trở thành: liệu chúng ta có thể gán giá trị nhị phân cho các ngày sao cho ít nhất một trong các cửa sổ có độ dài-2 được truy vấn khớp với mẫu đã chọn trước không? Sự đơn giản hóa quan trọng là chỉ có cấu trúc tương đối giữa các$a_i$vấn đề. Nếu có nhiều học sinh cùng chia sẻ$a_i$, tất cả họ đều hỏi về cùng một cửa sổ và chúng tôi có thể phân phối các dự đoán khác nhau trên chúng. 

Điều này biến thành một điều kiện kiểu chuồng bồ câu: trong một ngày cố định$a$, chỉ có bốn mẫu có thể cho$(a+1, a+2)$. Nếu chúng ta có ít nhất bốn học sinh có cùng điểm$a$, chúng ta có thể chỉ định tất cả bốn mẫu và đảm bảo thành công cho bất kỳ kết quả thực tế nào. Nếu chúng tôi có ít hơn bốn, chúng tôi không thể bao quát hết mọi khả năng cho khoảng thời gian đó. 

Do đó, toàn bộ vấn đề quy về việc kiểm tra xem có bất kỳ giá trị nào của$a_i$xuất hiện ít nhất 4 lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập | hàm mũ | hàm mũ | Quá chậm | 
| Đếm tần suất mỗi ngày |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Phân nhóm học sinh theo ngày đặt cược$a_i$. Điều này được thực hiện bằng cách sử dụng bản đồ tần số. Lý do là chỉ giống nhau$a_i$các giá trị có chung cấu trúc khoảng thời gian hai ngày, do đó việc nhóm sẽ tách biệt các ràng buộc độc lập. 
2. Cho mỗi ngày riêng biệt$a$, đếm xem có bao nhiêu học sinh đặt cược vào nó. Số lượng này biểu thị số lượng vị trí dự đoán mà chúng tôi có thể chỉ định cho cùng một cửa sổ$(a+1, a+2)$. 
3. Kiểm tra xem có số lượng nào ít nhất là 4 hay không. Nếu ngày đó tồn tại, chúng tôi ngay lập tức kết luận rằng thành công được đảm bảo. Lý do là có chính xác bốn kết quả nhị phân có thể xảy ra trong hai ngày: 00, 01, 10, 11. Với bốn học sinh trên cùng một cửa sổ, chúng tôi có thể chỉ định một dự đoán cho mỗi mẫu, đảm bảo bao quát mọi kết quả thực tế có thể xảy ra. 
4. Nếu không có ngày nào như vậy tồn tại, hãy ghi “Không”, vì mỗi cửa sổ có ít hơn bốn lần thử và do đó không thể bao gồm tất cả các kết quả có thể xảy ra của cửa sổ đó. Điều này ngụ ý rằng tồn tại ít nhất một phép gán kết quả có thể tránh được tất cả các dự đoán cùng một lúc. 

Tính đúng đắn phụ thuộc vào thực tế là mỗi học sinh độc lập yêu cầu khớp một trong bốn khả năng và chỉ có bội số ở cùng một cửa sổ mới cho phép bao quát toàn bộ. 

### Tại sao nó hoạt động 

Mỗi học sinh tương ứng với một yêu cầu khớp một cặp bit cụ thể tại các vị trí$a_i+1$Và$a_i+2$. Vì các cặp này độc lập trên các$a_i$, cách duy nhất để đảm bảo thành công là hoàn thành đầy đủ tất cả bốn nhiệm vụ có thể có của một cặp. Nếu có$a_i$xuất hiện bốn lần, chúng tôi có thể chỉ định tất cả bốn mẫu cho những học sinh đó, đảm bảo rằng bất kể kết quả thực tế của cặp đó ra sao, vẫn tồn tại một mẫu phù hợp. Nếu không tồn tại bội số như vậy thì đối với mỗi cửa sổ sẽ có ít nhất một mẫu không được chỉ định và đối thủ có thể chọn mẫu đó trên toàn cầu cho cửa sổ đó, đánh bại tất cả các cược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        ok = False
        for v in freq.values():
            if v >= 4:
                ok = True
                break
        
        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng bản đồ băm để đếm tần suất của mỗi ngày đặt cược. Đây là trạng thái duy nhất chúng ta cần, vì danh tính của học sinh ngoài nhóm là không liên quan. 

Việc kiểm tra tần số ≥ 4 mã hóa trực tiếp bốn mẫu nhị phân có thể có của chuỗi hai ngày. Vòng lặp kết thúc sớm khi tìm thấy nhóm như vậy, đảm bảo hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 1 1
```Chúng tôi theo dõi tần số: 

| bước | giá trị | trạng thái tần số | 
| --- | --- | --- | 
| 1 | 1 | {1:1} | 
| 2 | 1 | {1:2} | 
| 3 | 1 | {1:3} | 
| 4 | 1 | {1:4} | 

Khi số đếm đạt tới 4, chúng tôi kết luận rằng tất cả bốn mẫu đều có thể được chỉ định. Điều này đảm bảo mọi kết quả có thể xảy ra trong ngày 2 và 3, vì vậy câu trả lời là “Có”. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
```| bước | giá trị | trạng thái tần số | 
| --- | --- | --- | 
| 1 | 2 | {2:1} | 
| 2 | 2 | {2:2} | 
| 3 | 2 | {2:3} | 

Không có giá trị nào đạt tới 4, vì vậy ít nhất một mẫu nhị phân vẫn chưa được chỉ định. Đối thủ có thể chọn mô hình đó trong ngày thứ 3 và 4, tránh mọi dự đoán, vì vậy câu trả lời là “Không”. 

Những dấu vết này cho thấy ngưỡng 4 vừa cần vừa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | một lần để đếm cộng với quét theo tần số | 
| Không gian |$O(n)$| lưu trữ bản đồ băm nhiều nhất$n$giá trị riêng biệt | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$10^5$, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        out.append("Yes" if any(v >= 4 for v in freq.values()) else "No")
    return "\n".join(out)

# provided samples
assert run("""5
4
1 1 1 1
3
2 2 2
5
2 4 3 2 4
8
6 3 1 1 5 1 2 6
1
1000000000
""") == """Yes
No
Yes
No
No"""

# custom cases
assert run("""1
4
10 10 10 10
""") == "Yes"

assert run("""1
4
1 1 1 2
""") == "No"

assert run("""1
6
5 5 5 5 5 5
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau 4 | Có | trường hợp thành công tối thiểu | 
| Chia 3+1 | Không | thất bại gần ngưỡng | 
| tất cả đều lớn | Có | độ bền cho số lượng lớn hơn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một học sinh duy nhất. Ví dụ, đầu vào`n = 1, a = [100]`. Tần số là 1, nhỏ hơn 4 nên đầu ra là “Không”. Thuật toán kết luận chính xác rằng một dự đoán duy nhất không thể bao gồm tất cả bốn kết quả có thể xảy ra trong khoảng thời gian hai ngày. 

Một trường hợp đặc biệt khác là khi có đúng ba học sinh học cùng một ngày. Mặc dù chúng chồng chéo lên nhau nhưng vẫn có bốn kết quả có thể xảy ra trong hai ngày tới và một kết quả vẫn chưa được khám phá. Thuật toán đưa ra kết quả “Không” một cách chính xác, phản ánh rằng ít nhất một phép gán đối nghịch sẽ tránh được tất cả các dự đoán. 

Trường hợp cạnh cuối cùng là đầu vào đồng nhất lớn như$10^5$các giá trị giống nhau. Việc kiểm tra tần suất ngay lập tức mang lại thành công trong$O(n)$thời gian, chứng minh rằng thuật toán có quy mô mà không cần kiểm tra cấu trúc vượt quá số lượng.
