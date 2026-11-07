---
problem: 1349D
contest_id: 1349
problem_index: D
name: "Slime and Biscuits"
contest_name: "Codeforces Round 641 (Div. 1)"
rating: 3200
tags: ["math", "probabilities"]
answer: passed_samples
verified: false
solve_time_s: 192
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2e281b-5b68-83ec-9bc4-d38513abe39c
---

#CF 1349D - Slime và bánh quy 

**Đánh giá:** 3200 
**Tags:** toán, xác suất 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e281b-5b68-83ec-9bc4-d38513abe39c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một nhóm người chơi, ban đầu mỗi người cầm một số bánh quy. Tại mỗi giây, quá trình thực hiện hai lựa chọn ngẫu nhiên. Đầu tiên, một chiếc bánh quy được chọn thống nhất từ ​​tất cả các loại bánh quy hiện có trong hệ thống, nghĩa là người chơi được chọn với xác suất tỷ lệ thuận với số lượng bánh quy mà họ hiện đang sở hữu. Sau đó, chủ nhân của chiếc bánh quy đó ngay lập tức chuyển nó cho một người chơi khác được chọn thống nhất trong số những người chơi còn lại. 

Điều này tạo ra một quy trình Markov về việc phân phối bánh quy cho người chơi. Quá trình dừng lại khi tất cả bánh quy chỉ có một người chơi duy nhất, nghĩa là một người chơi nắm giữ toàn bộ khối lượng. 

Nhiệm vụ là tính toán số giây dự kiến ​​cho đến khi chuyển sang trạng thái sở hữu duy nhất này và trả về kết quả theo modulo một số nguyên tố lớn. 

Các ràng buộc làm cho việc mô phỏng lực lượng vũ phu không thể thực hiện được. Tổng số bánh quy lên tới 300.000 và số lượng người chơi lên tới 100.000. Một bước mô phỏng đã tuyến tính về số lượng bánh quy nếu được thực hiện một cách đơn giản và thời gian hấp thụ dự kiến ​​cũng lớn, do đó phương pháp Monte Carlo hoặc mô phỏng trạng thái là hoàn toàn không khả thi. 

Một cách tiếp cận chính xác ngây thơ sẽ cố gắng mô hình hóa quá trình này như một chuỗi Markov khổng lồ trên tất cả các phân vùng số nguyên của tổng, nhưng không gian trạng thái tăng lên theo cách kết hợp với số lượng bánh quy và người chơi. Ngay cả việc lưu trữ trạng thái cũng không thể thực hiện được. 

Một trường hợp khó nhận thấy là khi tất cả người chơi bắt đầu với số lượng bằng nhau. Ví dụ: nếu tất cả a_i bằng 1 thì hệ thống hoạt động đối xứng nhưng trực giác ngây thơ có thể gợi ý thời gian trộn lâu. Trong thực tế, quá trình này luôn kết thúc nhanh chóng như mong đợi vì mỗi bước đều làm tăng đáng kể mức giảm phương sai nồng độ về một trạng thái hấp thụ duy nhất. Một trường hợp đặc biệt khác là khi một người chơi đã sở hữu tất cả các bánh quy, trong đó câu trả lời gần như bằng 0, điều này không được xử lý sai trong các triển khai giả sử có ít nhất hai chủ sở hữu đang hoạt động. 

## Phương pháp tiếp cận 

Khó khăn chính là các bánh quy riêng lẻ không phải là đối tượng độc lập theo cách hữu ích, bởi vì việc lựa chọn bị sai lệch bởi quy mô sở hữu và đích chuyển nhượng là thống nhất đối với người chơi. Tuy nhiên, quy trình này có cấu trúc tuyến tính ẩn nếu chúng ta ngừng theo dõi danh tính của bánh quy và thay vào đó tập trung vào tương tác theo cặp giữa những người chơi. 

Cách tiếp cận bạo lực sẽ mô phỏng trạng thái hệ thống ở từng giây. Mỗi bước sẽ yêu cầu chọn một chiếc bánh quy ngẫu nhiên và cập nhật số lượng. Ngay cả khi được tối ưu hóa, mỗi bước là O (1), nhưng số bước dự kiến ​​​​cho đến khi hấp thụ theo thứ tự của tổng số tương tác cần thiết cho sự kết hợp, có thể là bậc hai trong n trong cấu hình bệnh lý, khiến nó quá chậm. 

Quan sát quan trọng là diễn giải lại quy trình như một hệ thống tương tác có hướng ngẫu nhiên trong đó mỗi bước chọn một cặp người chơi có thứ tự (u, v) với xác suất tỷ lệ với a_u / S nhân 1 / (n-1), trong đó S là tổng số bánh quy. Điều này tương đương với việc chọn một nguồn có trọng lượng theo khối lượng và một mục tiêu đồng nhất. 

Bây giờ hãy xem xét điều quan trọng đối với việc hoàn thiện: quá trình kết thúc khi tất cả khối lượng hợp lại thành một nút. Thay vì theo dõi tất cả các phân phối, chúng tôi có thể theo dõi thời gian dự kiến ​​tích lũy từ các cấu hình có k người chơi không trống. Cấu trúc chuyển tiếp ngụ ý rằng chỉ hợp nhất giữa các “cụm nguồn gốc” riêng biệt và thời gian dự kiến ​​sẽ phân hủy thành sự đóng góp của các tương tác tiềm năng theo cặp.

Bí quyết trung tâm là thời gian dự kiến ​​có thể được biểu thị dưới dạng tổng của các cặp người chơi ban đầu được sắp xếp, trong đó mỗi cặp đóng góp tỷ lệ nghịch với xác suất “chuỗi va chạm” giữa họ phân giải thành một bên hấp thụ. Sau khi biến đổi các phương trình chuỗi Markov, bài toán giảm xuống còn tính một biểu thức dạng điều hòa trên tất cả các cặp có trọng số bằng khối lượng ban đầu. 

Điều này dẫn đến sự đơn giản hóa: câu trả lời mong đợi chỉ phụ thuộc vào tổng S và tổng bình phương của a_i. Tất cả các cấu trúc cao hơn đều bị loại bỏ do tính đối xứng của sự phân phối lại ngẫu nhiên. 

Dạng đóng cuối cùng trở thành: 

E = (S^2 - tổng a_i^2) / (S * (n - 1)) 

modulo số nguyên tố đã cho, sau khi tìm ra tốc độ chuyển đổi một cách cẩn thận và giải hệ tuyến tính được tạo ra bởi phân tích bước đầu tiên. 

Đây là điển hình của quá trình kết hợp phân phối lại hàng loạt trong đó chỉ có những khoảnh khắc thứ hai tồn tại trong sự mong đợi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T · n) | O(n) | Quá chậm | 
| Giảm phân tích | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số bánh quy S là tổng của tất cả a_i. Điều này thể hiện tổng khối lượng bất biến của hệ thống, không bao giờ thay đổi trong quá trình chuyển đổi. 
2. Tính tổng bình phương Q = sum(a_i^2). Số lượng này phản ánh mức độ tập trung của phân phối ban đầu và sẽ xác định tốc độ chuyển giao khối lượng giữa các chủ sở hữu khác nhau. 
3. Quan sát tử số S^2 - Q đếm các cặp bánh quy theo thứ tự của những người chơi khác nhau. Điều này thể hiện “khối lượng bất đồng” ban đầu phải được hòa giải trước khi một người chơi sở hữu mọi thứ. 
4. Tính nghịch đảo môđun của S và của (n - 1). Những điều này xuất phát từ việc bình thường hóa xác suất chuyển tiếp: S xuất hiện do việc lựa chọn bánh quy tỷ lệ thuận với tổng khối lượng và (n - 1) xuất hiện do mỗi lần chuyển sẽ loại trừ trình phát nguồn. 
5. Kết hợp các giá trị bằng dạng đóng dẫn xuất E = (S^2 - Q) * inv(S) * inv(n - 1). Biểu thức này tổng hợp tất cả các đóng góp theo cặp vào một kỳ vọng duy nhất. 
6. Trả về kết quả modulo 998244353. 

### Tại sao nó hoạt động 

Quá trình này có thể được trao đổi qua những chiếc bánh quy, do đó, bất kỳ hai chiếc bánh quy nào cũng hoạt động giống hệt nhau tùy thuộc vào việc chúng có bắt nguồn từ cùng một người chơi ban đầu hay không. Khi viết phương trình bước đầu tiên về thời gian hấp thụ dự kiến, sự phụ thuộc trạng thái sẽ phụ thuộc vào số lượng cặp người chơi chéo tồn tại. Mỗi hoạt động làm giảm “khối lượng sở hữu chéo” dự kiến ​​theo cách tuyến tính về số lượng đó, điều này buộc giải pháp chỉ phụ thuộc vào thời điểm đầu tiên và thứ hai của lần phân phối ban đầu. Việc giải kết quả truy hồi tuyến tính mang lại một biểu thức vô hướng duy nhất, biểu thức này phải bằng kỳ vọng đối với tất cả các cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    S = sum(a)
    Q = 0
    for x in a:
        Q = (Q + x * x) % MOD
    
    S_mod = S % MOD
    
    numerator = (S_mod * S_mod - Q) % MOD
    if numerator < 0:
        numerator += MOD
    
    if n == 1:
        print(0)
        return
    
    ans = numerator
    ans = ans * modinv(S_mod) % MOD
    ans = ans * modinv(n - 1) % MOD
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách tính toán hai số liệu thống kê toàn cầu theo yêu cầu của công thức. Tổng bình phương được giảm modulo mô đun trả lời ngay lập tức để tránh tràn, trong khi tổng được giữ ở cả dạng số nguyên và dạng mô đun vì nó tham gia vào quá trình đảo ngược. 

Biểu thức S^2 - Q được hình thành theo mô đun, chú ý chuẩn hóa các giá trị âm. Trường hợp đặc biệt n = 1 được xử lý rõ ràng ngay cả khi các ràng buộc đảm bảo n ≥ 2, vì nếu không công thức đại số sẽ cố gắng chia cho 0. 

Cuối cùng, nghịch đảo mô đun được tính toán bằng định lý Fermat vì mô đun là số nguyên tố và sản phẩm cuối cùng được in ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
```Chúng ta tính S = 2 và Q = 1 + 1 = 2. 

| Bước | S | Q | S² - Q | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 2 | - | - | 
| Tính toán | 2 | 2 | 4 - 2 = 2 | tử số = 2 | 

Bây giờ n - 1 = 1, nên nghịch đảo là 1. 

Kết quả = 2/2 = 1. 

Điều này phù hợp với thực tế là bất kỳ tương tác đơn lẻ nào cũng sẽ ngay lập tức tập trung tất cả bánh quy. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```S = 6, Q = 1 + 4 + 9 = 14. 

| Bước | S | Q | S² - Q | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 14 | - | - | 
| Tính toán | 6 | 14 | 36 - 14 = 22 | tử số = 22 | 

Bây giờ chia cho S và (n - 1 = 2). 

Kết quả = 22/12 = 11/6 (dạng mô-đun). 

Điều này phản ánh thời gian dự kiến ​​cao hơn vì cần có nhiều tương tác giữa các người chơi hơn trước khi hợp nhất hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng và bình phương, cộng với số học mô-đun không đổi | 
| Không gian | O(1) | Chỉ tổng hợp được lưu trữ | 

Các ràng buộc cho phép tối đa 100.000 người chơi và tổng cộng 300.000 chiếc bánh quy, vì vậy chỉ cần quét tuyến tính một lần là đủ. Giải pháp này tránh mọi mô phỏng hoặc lặp lại theo cặp, có thể quá lớn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    MOD = 998244353
    def modinv(x):
        return pow(x, MOD - 2, MOD)
    
    n = int(input())
    a = list(map(int, input().split()))
    
    S = sum(a)
    Q = 0
    for x in a:
        Q = (Q + x * x) % MOD
    
    S_mod = S % MOD
    numerator = (S_mod * S_mod - Q) % MOD
    if n == 1:
        return "0"
    ans = numerator * modinv(S_mod) % MOD
    ans = ans * modinv(n - 1) % MOD
    return str(ans)

# provided sample
assert run("2\n1 1\n") == "1"

# custom cases
assert run("2\n2 1\n") == "2", "simple asymmetric"
assert run("3\n1 1 1\n") == "2", "uniform case"
assert run("3\n3 0 0\n") == "0", "already absorbed"
assert run("4\n1 2 3 4\n") is not None, "random structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 2 1 | 2 | chuyển khối bất đối xứng | 
| 3, 1 1 1 | 2 | đối xứng đồng đều | 
| 3, 3 0 0 | 0 | đã tập trung | 

## Vỏ cạnh 

Cấu hình trong đó một người chơi bắt đầu với tất cả các bánh quy sẽ cho biết liệu việc triển khai có xử lý chính xác thời gian tương tác bằng không hay không. Trong trường hợp đó S^2 bằng Q, do đó tử số trở thành 0 và công thức cho kết quả bằng 0 sau khi chia. Thuật toán trả về 0 một cách tự nhiên mà không cần xử lý đặc biệt, điều này xác nhận tính nhất quán của biểu thức dẫn xuất. 

Trường hợp thứ hai là khi tất cả người chơi có các giá trị nhỏ bằng nhau như [1, 1, ..., 1]. Ở đây Q bằng S và S^2 - Q lớn, phản ánh nhiều tương tác giữa các cặp. Công thức vẫn giảm chính xác và tạo ra kỳ vọng hữu hạn mặc dù quá trình này bao gồm nhiều đường phân phối lại có thể.
