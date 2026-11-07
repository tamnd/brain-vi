---
problem: 1370C
contest_id: 1370
problem_index: C
name: "Number Game"
contest_name: "Codeforces Round 651 (Div. 2)"
rating: 1400
tags: ["games", "math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 243
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e52c3-f374-83ec-8a3c-fb04c12ded07
---

#CF 1370C - Trò Chơi Số 

**Đánh giá:** 1400 
**Tags:** trò chơi, toán học, lý thuyết số 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e52c3-f374-83ec-8a3c-fb04c12ded07 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang chơi trò chơi theo lượt hai người chơi bắt đầu từ một số nguyên duy nhất. Trong mỗi lần di chuyển, người chơi hiện tại sẽ biến đổi số bằng một trong hai thao tác: trừ một số cho số đó hoặc chia số đó cho một số ước lẻ lớn hơn một. Số chia thực sự phải chia số hiện tại và các số chia bao gồm chính số đó, do đó, chia cho số đầy đủ luôn được phép khi số lớn hơn một. 

Trò chơi kết thúc khi người chơi không thể thực hiện bất kỳ động thái nào, điều này chỉ xảy ra khi số trở thành 1. Ai gặp phải tình huống đó sẽ thua ngay lập tức vì không thể trừ hay chia được nữa. 

Nhiệm vụ là xác định, với mỗi giá trị ban đầu của n, liệu người chơi đầu tiên có bị buộc phải thắng hay không nếu giả sử cả hai bên đều chơi tối ưu. 

Các ràng buộc cho phép n tối đa 10^9 và tối đa 100 trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ mọi tìm kiếm trong không gian trạng thái trên tất cả các số có thể truy cập được. Ngay cả việc lập trình động đơn giản trên tất cả các trạng thái lên đến n cũng không thể thực hiện được vì n quá lớn. Bất kỳ giải pháp nào cũng phải giảm bớt vấn đề xuống một vài phép kiểm tra số học và các thuộc tính cấu trúc của số. 

Một cạm bẫy ngây thơ nhưng phổ biến là cho rằng trò chơi chỉ phụ thuộc vào tính chẵn lẻ hoặc chỉ vào số nước đi cần thiết để đạt đến 1. Ví dụ, nghĩ rằng “số lẻ luôn tốt” sẽ thất bại ngay lập tức vì n = 9 và n = 3 hành xử khác nhau: 3 thắng, nhưng 9 hành xử khác khi chơi tối ưu do có nhiều ước. Một trường hợp thất bại tinh tế khác là coi phép trừ luôn chiếm ưu thế; trong thực tế, phép chia cho các thừa số lẻ có thể bỏ qua phần lớn không gian trạng thái và thay đổi hoàn toàn lý luận tính chẵn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng trò chơi dưới dạng tối đa đệ quy trên tất cả các nước đi có thể có. Từ một n cho trước, chúng ta phân nhánh thành tất cả các trạng thái n - 1 và n / d với mọi ước số lẻ d > 1. Điều này xác định một đồ thị trò chơi trên các số nguyên lên đến n. Mặc dù mỗi trạng thái chỉ có ước số O(sqrt n), độ sâu đệ quy có thể là O(n) và các trạng thái lặp lại xuất hiện theo những cách phức tạp. Trong trường hợp xấu nhất, điều này dẫn đến việc khám phá theo cấp số nhân, điều này không thể thực hiện được ngay cả với n nhỏ như 10^9. 

Quan sát quan trọng là cấu trúc của các nước đi hợp lệ sẽ thu gọn hầu hết các trường hợp thành một số ít các phạm trù số học. Các số lẻ hoạt động theo cách rất đơn giản vì bất kỳ tổng số lẻ nào cũng có ước số lẻ lớn hơn 1 và có thể ngay lập tức giảm nó thành số lẻ nhỏ hơn. Các số chẵn hoạt động khác nhau tùy thuộc vào số lượng ước số của 2 mà chúng chứa. Vị trí thực sự khó khăn duy nhất là khi số đó là lũy thừa của hai, vì nó không có ước số lẻ lớn hơn 1, buộc trò chơi phải hoạt động giống như một chuỗi giảm dần đơn giản. Một cấu trúc đặc biệt khác phát sinh khi n hai lần là một số nguyên tố lẻ, trong đó phép chia tạo ra một trạng thái nguyên tố hoạt động khác với các số chẵn nói chung. 

Giải pháp cuối cùng giảm toàn bộ trò chơi xuống việc kiểm tra một vài trường hợp cấu trúc trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu Minimax | Hàm mũ | O(n) trạng thái đệ quy | Quá chậm | 
| Phân tích trường hợp kết cấu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nếu n bằng 1, người chơi đầu tiên không được di chuyển và thua ngay. Đây là vị trí cuối cùng duy nhất trong trò chơi. 
2. Nếu n bằng 2 thì nước đi duy nhất là trừ 1, đối phương còn 1, bảo đảm người đi đầu sẽ thắng. 
3. Nếu n lẻ và lớn hơn 1, người chơi đầu tiên luôn có thể chia cho chính n, vì n là ước số lẻ lớn hơn 1. Nước đi này sẽ đưa trò chơi trực tiếp về 1, buộc phải thắng. 
4. Nếu n chẵn, phân tích nó thành n = 2^k * m trong đó m lẻ. Hành vi phụ thuộc vào k và liệu m có bằng 1 hay không. 
5. Nếu n là lũy thừa của hai (m = 1), mỗi nước đi buộc phải trừ đi một lần liên tục, do đó kết quả hoàn toàn phụ thuộc vào tính chẵn lẻ của k. Trong tình huống này, người chơi đầu tiên thua khi k > 1 vì cuối cùng họ sẽ giao một lũy thừa nhỏ hơn bằng 2, vẫn chẵn nhưng gần bằng 1 trong cấu hình thua. 
6. Nếu n có đúng một thừa số 2 (k = 1) thì n = 2 * m. Nếu m là số nguyên tố, người chơi thứ hai luôn có thể phản ứng theo cách duy trì cấu trúc chiến thắng, khiến người chơi thứ nhất rơi vào thế thua. Nếu m là hợp số, người chơi đầu tiên có thể chia cho hệ số lẻ để tạo ra trạng thái chẵn nhỏ hơn và giành quyền kiểm soát. 

Cái nhìn sâu sắc mang tính quyết định là mọi vị trí đều giảm xuống thành thắng ngay lập tức (lẻ n > 1), thắng tầm thường (n = 2) hoặc các trường hợp thua có cấu trúc trong đó các số chẵn không thể bị phá vỡ một cách thuận lợi. Cấu trúc không tầm thường duy nhất là cách hệ số hóa của 2 tương tác với sự sẵn có của các ước số lẻ. 

### Tại sao nó hoạt động 

Trò chơi hoàn toàn được xác định bằng việc người chơi có thể giảm thành phần lẻ của số đến mức nào và liệu bất kỳ động thái phân nhánh nào có thể tránh khiến đối thủ có một chuỗi giảm rõ ràng hay không. Các số lẻ luôn làm đối phương mất vị trí vì chúng chứa đựng một nước đi tự hủy về 1. Các số chẵn chỉ nguy hiểm khi chúng giới hạn các ước số lẻ trong một thừa số nguyên tố duy nhất, buộc trò chơi rơi vào tình trạng đếm ngược tuyến tính. Điều này thu gọn toàn bộ biểu đồ trạng thái thành một tập hợp nhỏ các lớp tương đương, đảm bảo không tồn tại các chu kỳ ẩn lâu hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False
    if x % 2 == 0:
        return x == 2
    i = 3
    while i * i <= x:
        if x % i == 0:
            return False
        i += 2
    return True

t = int(input())
for _ in range(t):
    n = int(input())

    if n == 1:
        print("FastestFinger")
    elif n == 2:
        print("Ashishgup")
    elif n % 2 == 1:
        print("Ashishgup")
    else:
        # n is even and > 2
        # check if power of two
        if (n & (n - 1)) == 0:
            print("FastestFinger")
        else:
            # remove factor 2
            m = n
            while m % 2 == 0:
                m //= 2

            # if odd part is 1 => power of two already handled, so safe
            if m == 1:
                print("FastestFinger")
            else:
                # if m is prime AND n has no extra structure
                if is_prime(m) and n // m == 2:
                    print("FastestFinger")
                else:
                    print("Ashishgup")
```Đầu tiên, mã sẽ tách biệt các trường hợp đầu cuối tầm thường: n bằng 1 và n bằng 2. Sau đó, tất cả các số lẻ sẽ thắng ngay lập tức vì việc chia cho chính số đó luôn hợp pháp và kết thúc trò chơi. 

Đối với số chẵn, thủ thuật bit`(n & (n - 1)) == 0`phát hiện sức mạnh thuần túy của hai người, đang mất vị trí kể từ khi trò chơi thoái hóa thành phép trừ lặp đi lặp lại cho đến khi đạt 1 khi chơi bắt buộc. Nếu số đó không phải là lũy thừa của hai, chúng ta sẽ trích ra thành phần lẻ m của nó. 

Sự khác biệt quan trọng là liệu m có phải là số nguyên tố hay không và số đó có đúng bằng 2 lần số nguyên tố đó hay không. Cấu hình đó ngăn chặn bất kỳ động thái chia số lẻ có lợi nào cho người chơi đầu tiên, khiến trò chơi rơi vào cấu trúc thua cuộc. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào đại diện từ bộ mẫu. 

### Ví dụ 1 

Đầu vào: n = 6 

| Bước | n | Loại | Di chuyển | Kết quả n | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | thậm chí tổng hợp | chia cho 3 | 2 | 
| 2 | 2 | trường hợp cơ sở | trừ 1 | 1 | 
| 3 | 1 | thiết bị đầu cuối | không | thua cho người chơi hiện tại | 

Điều này thể hiện người chơi đầu tiên buộc giảm xuống còn 2, sau đó đối thủ buộc phải đi nước thua. Điều quan trọng là sự tồn tại của ước số lẻ 3. 

### Ví dụ 2 

Đầu vào: n = 4 

| Bước | n | Loại | Di chuyển | Kết quả n | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | sức mạnh của hai | trừ 1 | 3 | 
| 2 | 3 | số nguyên tố lẻ | chia cho 3 | 1 | 
| 3 | 1 | thiết bị đầu cuối | không | người chơi trước thua | 

Ở đây, người chơi đầu tiên không thể tránh khỏi việc đưa cho đối thủ một câu trả lời thắng lợi. Bất kể cấu trúc trừ hay bắt buộc, người chơi thứ hai luôn có thể buộc phải thắng. 

Sự tương phản giữa 6 và 4 làm nổi bật rằng không phải tất cả các số chẵn đều hoạt động giống nhau; cấu trúc phân chia quan trọng hơn độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt n) cho mỗi lần kiểm tra trường hợp xấu nhất | tính nguyên tố và trích nhân tử cho thành phần lẻ | 
| Không gian | O(1) | chỉ có các biến phụ không đổi | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với n tối đa 10^9, do đó, ngay cả kiểm tra sqrt(n) cũng đủ nhanh trong Python. Tất cả các hoạt động khác là kiểm tra số học hoặc bit thời gian không đổi, giữ cho giải pháp luôn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def is_prime(x):
        if x < 2:
            return False
        if x % 2 == 0:
            return x == 2
        i = 3
        while i * i <= x:
            if x % i == 0:
                return False
            i += 2
        return True

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n == 1:
            out.append("FastestFinger")
        elif n == 2:
            out.append("Ashishgup")
        elif n % 2 == 1:
            out.append("Ashishgup")
        else:
            if (n & (n - 1)) == 0:
                out.append("FastestFinger")
            else:
                m = n
                while m % 2 == 0:
                    m //= 2
                if m == 1:
                    out.append("FastestFinger")
                elif is_prime(m) and n // m == 2:
                    out.append("FastestFinger")
                else:
                    out.append("Ashishgup")
    return "\n".join(out)

# provided samples
assert run("7\n1\n2\n3\n4\n5\n6\n12\n") == "FastestFinger\nAshishgup\nAshishgup\nFastestFinger\nAshishgup\nFastestFinger\nAshishgup"

# custom cases
assert run("1\n1\n") == "FastestFinger", "minimum case"
assert run("1\n2\n") == "Ashishgup", "small win"
assert run("1\n8\n") == "FastestFinger", "power of two losing chain"
assert run("1\n9\n") == "Ashishgup", "odd composite immediate win"
assert run("1\n18\n") == "Ashishgup", "even composite structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Ngón tay nhanh nhất | trạng thái mất thiết bị đầu cuối | 
| 2 | Ashishgup | trường hợp thắng nhỏ nhất | 
| 8 | Ngón tay nhanh nhất | sức mạnh của hai hành vi | 
| 9 | Ashishgup | lẻ tổng hợp thắng liền | 
| 18 | Ashishgup | xử lý đồng đều tổng hợp | 

## Vỏ cạnh 

Với n = 1, thuật toán ngay lập tức phân loại nó là thua vì không có nước đi nào tồn tại. Đầu ra là FastestFinger, phù hợp với quy tắc người chơi di chuyển sẽ thua khi bị mắc kẹt. 

Với n = 2, mã coi nó như một trường hợp chiến thắng đặc biệt. Nước đi duy nhất là phép trừ, buộc đối phương phải có n = 1, bảo đảm thắng lợi. 

Với n = 8, giá trị là lũy thừa thuần túy của hai. Kiểm tra bit`(n & (n - 1)) == 0`kích hoạt, gửi nó đến danh mục thua cuộc. Lý do là không tồn tại ước số lẻ nên trò chơi thoái hóa thành phép trừ bắt buộc. 

Với n = 9 là số lẻ và lớn hơn 1 nên người chơi đầu tiên chia cho 9 ngay lập tức và thắng một nước đi, phù hợp với phân loại. 

Với n = 18, số này là số chẵn nhưng không phải là lũy thừa thuần túy của 2 và thành phần lẻ của nó là 9, là hợp số. Thuật toán xác định đó là một vị trí chiến thắng vì tồn tại một ước số lẻ có lợi, cho phép người chơi đầu tiên buộc đối thủ phải giảm cấu trúc thua.
