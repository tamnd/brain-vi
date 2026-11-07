---
problem: 1358A
contest_id: 1358
problem_index: A
name: "Park Lighting"
contest_name: "Codeforces Round 645 (Div. 2)"
rating: 800
tags: ["greedy", "math"]
answer: passed_samples
verified: true
solve_time_s: 190
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e389b-22c4-83ec-a133-59a777b0e455
---

#CF 1358A - Chiếu sáng công viên 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e389b-22c4-83ec-a133-59a777b0e455 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$, trong đó mỗi ô đại diện cho một ô vuông trong công viên. Lưới được bao quanh bởi các đường phố và cũng có các đường phố nằm giữa mỗi cặp ô liền kề theo chiều ngang và chiều dọc. Đèn lồng có thể được đặt trên các đoạn phố này và mỗi đèn lồng sẽ chiếu sáng hai ô liền kề với con phố nơi nó được đặt. Nếu đèn lồng nằm ở ranh giới ngoài của lưới thì nó chỉ chiếu sáng một ô duy nhất vì chỉ có một ô vuông liền kề. 

Nhiệm vụ là xác định số lượng đèn lồng tối thiểu cần thiết để mỗi ô trong lưới được chiếu sáng bởi ít nhất một đèn lồng. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và mỗi kích thước lưới có thể lớn bằng$10^4$. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc xây dựng lưới trên mỗi ô. Bất kỳ giải pháp nào cũng phải hoạt động trong thời gian không đổi cho mỗi trường hợp thử nghiệm, vì ngay cả$O(nm)$mỗi trường hợp thử nghiệm sẽ bùng nổ$10^8$hoạt động cho mỗi thử nghiệm trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi một hoặc cả hai chiều đều rất nhỏ. trong một$1 \times 1$lưới, cần một chiếc đèn lồng duy nhất. trong một$1 \times m$hoặc$n \times 1$dải, mỗi đèn lồng bao phủ tối đa hai ô, nhưng hành vi ranh giới làm giảm hiệu quả và phải áp dụng đối số ghép đôi ngây thơ một cách cẩn thận. Ví dụ, trong$1 \times 3$, câu trả lời là 2, không phải 1, vì một chiếc đèn lồng che tối đa hai ô liền kề và ô thứ ba vẫn chưa được che chắn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng việc đặt đèn lồng và đánh dấu các ô được chiếu sáng, có thể thử tất cả các vị trí hoặc tham lam lấp đầy các ô không được che chắn. Người ta có thể tưởng tượng việc lặp đi lặp lại từng đoạn đường và đặt đèn lồng cho đến khi tất cả các ô đều được thắp sáng. Điều này đòi hỏi phải duy trì một mạng lưới phủ sóng và liên tục quét các ô chưa được phát hiện, dẫn đến ít nhất$O(nm)$hoạt động trên mỗi trường hợp thử nghiệm và có thể tệ hơn nếu chúng tôi thử bất kỳ tìm kiếm hoặc tối ưu hóa nào trên các vị trí. 

Điều này không thành công vì lưới có thể cực kỳ lớn và số lượng ca kiểm thử cũng lớn. Cấu trúc của bài toán rất đều đặn: mỗi chiếc đèn lồng luôn bao phủ chính xác hai ô liền kề trừ khi nó nằm trên ranh giới. Điều này có nghĩa là phạm vi phủ sóng về cơ bản là về việc ghép nối các ô một cách hiệu quả. 

Quan sát quan trọng là chúng tôi muốn bao gồm tất cả$n \times m$các ô bằng cách sử dụng các đối tượng mà mỗi đối tượng bao phủ tối đa 2 ô. Nếu chúng ta bỏ qua ranh giới trong giây lát, chúng ta sẽ cần khoảng$\lceil \frac{nm}{2} \rceil$đèn lồng. Tuy nhiên, vì đèn lồng bị giới hạn ở các cạnh của lưới nên chúng ta không thể tự do ghép các ô tùy ý, nhưng cấu trúc lưới vẫn cho phép xây dựng tối ưu đạt được chính xác$\left\lceil \frac{n \cdot m}{2} \right\rceil$. 

Điều này thoạt nhìn không rõ ràng, nhưng lưới có thể được phân tách thành các cặp liền kề rời rạc theo kiểu bàn cờ. Mỗi đèn lồng bao phủ một cách hiệu quả một cạnh của biểu đồ lưới và biểu đồ lưới là lưỡng cực với sự kết hợp hoàn hảo hoặc gần hoàn hảo tùy thuộc vào tính chẵn lẻ. Số cạnh tối thiểu cần thiết để bao phủ tất cả các đỉnh tương ứng với việc ghép tất cả các ô ngoại trừ một ô, chỉ xuất hiện khi$n \cdot m$thật kỳ quặc. 

Vì vậy, vấn đề giảm xuống còn việc đếm xem chúng ta có thể tạo thành bao nhiêu cặp ô, chính xác là mức trần của một nửa số ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nm)$mỗi bài kiểm tra |$O(nm)$| Quá chậm | 
| Cái nhìn sâu sắc về ghép nối tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$m$. Chúng xác định tổng số ô$n \cdot m$, mà chúng tôi muốn che phủ bằng cách sử dụng đèn lồng chiếu sáng tối đa hai ô. 
2. Tính tổng số ô như sau$c = n \cdot m$. Đây là số lượng cốt lõi vì mỗi đèn lồng đóng góp tối đa hai đơn vị phủ sóng. 
3. Nếu$c$chẵn, trở về$c / 2$. Mỗi ô có thể được ghép nối hoàn hảo với một ô khác, nghĩa là mỗi đèn lồng bao phủ chính xác một cặp. 
4. Nếu$c$thật kỳ lạ, hãy quay lại$c / 2 + 1$. Một ô vẫn chưa được ghép nối và nó yêu cầu một vị trí đèn lồng chuyên dụng để chỉ che phủ một ô một cách hiệu quả. 

Tính chính xác phụ thuộc vào thực tế là lưới cho phép khớp đầy đủ các ô liền kề theo màu bàn cờ. Các ô liền kề luôn có tính chẵn lẻ đối lập nhau, do đó việc ghép nối luôn khả thi về mặt cấu trúc trên toàn lưới. 

## Tại sao nó hoạt động 

Lưới có thể được xem dưới dạng biểu đồ lưỡng cực trong đó mỗi ô là một nút và các cạnh kết nối các lân cận trực giao. Mỗi đèn lồng tương ứng với việc chọn một cạnh, bao phủ hai điểm cuối của nó. Mục đích là chọn số cạnh tối thiểu sao cho mọi nút đều liên quan đến ít nhất một cạnh được chọn. 

Trên biểu đồ lưới hai bên, luôn có thể tìm thấy một kết quả phù hợp bao gồm tất cả các đỉnh ngoại trừ một đỉnh khi tổng số đỉnh là số lẻ. Điều này đảm bảo rằng vùng phủ sóng tốt nhất có thể đạt được sẽ kết hợp tất cả trừ nhiều nhất là một ô. Do đó câu trả lời chỉ phụ thuộc vào tính chẵn lẻ của$n \cdot m$, cho$\lceil \frac{n \cdot m}{2} \rceil$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    print((n * m + 1) // 2)
```Việc thực hiện tính toán trực tiếp số ô và áp dụng công thức chia trần. biểu hiện$(n * m + 1) // 2$tránh các phép toán dấu phẩy động và xử lý chính xác cả trường hợp chẵn và lẻ. 

Điểm tinh tế duy nhất là sử dụng số học số nguyên thay vì phép chia, điều này đảm bảo tính chính xác cho các đầu vào lớn và tránh các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp:$1 \times 3$Và$3 \times 3$. 

Vì$1 \times 3$, tổng số ô là 3. 

| Bước | n | m | n·m | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 1 | 3 | 3 | (3 + 1)//2 | 2 | 

Điều này cho thấy rằng mặc dù chúng ta có thể cố gắng đặt một chiếc đèn lồng che phủ hai ô, nhưng vẫn còn một ô, cần có chiếc đèn lồng thứ hai. 

Vì$3 \times 3$, tổng số ô là 9. 

| Bước | n | m | n·m | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 3 | 3 | 9 | (9 + 1)//2 | 5 | 

Điều này xác nhận rằng một lưới có kích thước lẻ luôn để lại một ô không được che chắn trong bất kỳ sơ đồ ghép nối hoàn hảo nào, cần có thêm một đèn lồng. 

Những dấu vết này xác nhận rằng giải pháp chỉ phụ thuộc vào tính chẵn lẻ chứ không phụ thuộc vào hình học ngoài cấu trúc ghép đôi lưỡng cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ các phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$10^4$trường hợp thử nghiệm chỉ yêu cầu$10^4$tính toán liên tục theo thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str((n * m + 1) // 2))
    return "\n".join(out)

# provided samples
assert run("5\n1 1\n1 3\n2 2\n3 3\n5 3\n") == "1\n2\n2\n5\n8"

# minimum grid
assert run("1\n1 1\n") == "1"

# single row
assert run("1\n1 5\n") == "3"

# single column
assert run("1\n4 1\n") == "2"

# even grid
assert run("1\n2 4\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | lưới nhỏ nhất | 
| 1 5 | 3 | ghép nối hàng đơn | 
| 4 1 | 2 | ghép cột đơn | 
| 2 4 | 4 | thậm chí lưới ghép nối hoàn hảo | 

## Vỏ cạnh 

Đối với một$1 \times 1$lưới, chỉ có một ô. Thuật toán tính toán$(1 \cdot 1 + 1)//2 = 1$, điều này phù hợp với thực tế là cần ít nhất một chiếc đèn lồng mặc dù nó chỉ thắp sáng một ô duy nhất. 

Đối với một$1 \times 3$lưới, tính toán cho$(3 + 1)//2 = 2$. Về mặt khái niệm, chỉ có thể hình thành một cặp, để lại một ô không được che chắn, điều này buộc phải có một chiếc đèn lồng thứ hai. 

Đối với một$2 \times 2$lưới, tính toán cho$(4 + 1)//2 = 2$. Lưới có thể được phân chia hoàn hảo thành hai cặp liền kề, do đó, mỗi ô được bao phủ chính xác bằng hai đèn lồng, xác nhận việc ghép nối tối ưu mà không lãng phí.
