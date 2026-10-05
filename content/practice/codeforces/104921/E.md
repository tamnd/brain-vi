---
title: "CF 104921E - Trò chơi với số nguyên"
description: "Chúng tôi bắt đầu với một số nguyên duy nhất và hai người chơi thay phiên nhau sửa đổi nó. Trên mỗi nước đi, người chơi có thể tăng hoặc giảm giá trị hiện tại đúng một. Vanya di chuyển đầu tiên. Trò chơi kết thúc sớm nếu ngay sau khi Vanya thực hiện một nước đi, số kết quả chia hết cho 3."
date: "2026-06-28T18:08:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104921
codeforces_index: "E"
codeforces_contest_name: "Easy_Training"
rating: 0
weight: 104921
solve_time_s: 72
verified: false
draft: false
---

[CF 104921E - Trò chơi với số nguyên](https://codeforces.com/problemset/problem/104921/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một số nguyên duy nhất và hai người chơi thay phiên nhau sửa đổi nó. Trên mỗi nước đi, người chơi có thể tăng hoặc giảm giá trị hiện tại đúng một. Vanya di chuyển đầu tiên. Trò chơi kết thúc sớm nếu ngay sau khi Vanya thực hiện một nước đi, số kết quả chia hết cho 3. Nếu không có khoảnh khắc nào như vậy xảy ra trong tổng số 10 nước đi, Vova được tuyên bố là người chiến thắng. 

Khía cạnh mấu chốt là chỉ có Vanya mới có thể trực tiếp giành chiến thắng và chỉ nhờ vào nước đi của chính mình. Vai trò của Vova hoàn toàn là phòng thủ: anh ta cố gắng tránh cho phép Vanya tiếp đất theo bội số của 3 ngay sau lượt của Vanya, đồng thời sống sót đủ lâu để hết giới hạn 10 nước đi. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và giá trị ban đầu lên tới 1000. Điều này ngay lập tức loại trừ mọi nhu cầu mô phỏng nặng nề trên các không gian trạng thái lớn hoặc các cấu trúc tối ưu hóa. Về nguyên tắc, ngay cả việc khám phá cây trò chơi đơn giản cũng khả thi vì độ sâu bị giới hạn bởi 10 nước đi, nhưng chúng ta sẽ thấy rằng ngay cả điều đó cũng là quá mức cần thiết. 

Một điểm tinh tế là điều kiện thắng chỉ được kiểm tra sau nước đi của Vanya. Vova không bao giờ thắng trực tiếp bằng cách đạt đến khả năng chia hết, anh ta chỉ thắng bằng cách ngăn chặn thành công của Vanya cho đến khi đạt đến giới hạn nước đi. Sự bất đối xứng này là yếu tố thúc đẩy giải pháp. 

Một sai lầm ngây thơ là coi trò chơi là đối xứng hoặc kiểm tra khả năng chia hết sau mỗi nước đi. Ví dụ: bắt đầu từ n = 1, nếu Vanya chơi +1, chúng ta nhận được 2, không chia hết cho 3. Nếu một người kiểm tra sai cả hai người chơi, người ta có thể nghĩ Vova cũng có điều kiện thắng, điều này là sai. 

Một cạm bẫy phổ biến khác là mô phỏng một cách tham lam, luôn cố gắng tiến tới bội số của 3. Điều đó không thành công vì Vova chủ động can thiệp và vấn đề về cơ bản là đối nghịch. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng trò chơi như một cây tìm kiếm có giới hạn độ sâu. Mỗi trạng thái bao gồm số hiện tại, chỉ số di chuyển và lượt của nó. Từ mỗi trạng thái, chúng tôi chia thành hai khả năng, cộng hoặc trừ một khả năng. Chúng tôi dừng lại nếu đạt đến trạng thái mà Vanya vừa di chuyển và số đó chia hết cho 3 hoặc nếu chúng tôi vượt quá 10 bước. 

Lực lượng vũ phu này hoạt động vì không gian trạng thái rất nhỏ: tối đa 2 lựa chọn cho mỗi lần di chuyển trong 10 lần di chuyển sẽ mang lại nhiều nhất 2^10 = 1024 đường dẫn. Với 100 trường hợp thử nghiệm, con số này là khoảng 100.000 trạng thái và vẫn có thể quản lý được. 

Tuy nhiên, điều này là không cần thiết vì cấu trúc của bài toán thu gọn thành số học mô-đun. Mỗi bước di chuyển sẽ thay đổi số đi ±1, vì vậy chỉ có phần dư modulo 3 là quan trọng. Mỗi bước di chuyển sẽ lật phần dư theo một cách có thể đoán trước được: từ bất kỳ phần dư nào, Vanya luôn có thể buộc chuyển đổi và Vova chỉ có thể đáp lại bằng cách chuyển phần dư đó đi. 

Quan sát quan trọng là điều quan trọng duy nhất là liệu Vanya có thể buộc phần dư về 0 khi di chuyển trong vòng 10 bước hay không. Vì cả hai người chơi luôn thay đổi số bằng ±1 nên phần dư sẽ chuyển qua 0, 1, 2 một cách có kiểm soát. Điều này biến trò chơi thành một bài toán trạng thái tuần hoàn, hữu hạn thay vì một bài toán số ngày càng tăng. 

Nếu chúng ta thử tất cả các khả năng trong 10 nước đi, chúng ta sẽ nhanh chóng nhận thấy một mô hình: Vanya có thể thắng ngay lập tức bất cứ khi nào giá trị ban đầu chưa được bảo vệ bởi mô hình phản ứng của Vova. Trên thực tế, cách chơi tối ưu giảm xuống điều kiện chẵn lẻ/modulo đơn giản hơn là tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | O(2^10 · t) | O(10) | Được chấp nhận nhưng không cần thiết | 
| Phân tích mô-đun | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng trung tâm là chỉ theo dõi giá trị modulo 3 và lý do làm thế nào Vanya có thể giành được vị trí chiến thắng.

1. Tính số dư r = n mod 3. Đây là thông tin duy nhất ảnh hưởng đến việc liệu chúng ta có thể đạt được bội số của 3 sau nước đi của Vanya hay không. 
2. Quan sát rằng sau khi Vanya di chuyển, anh ấy muốn giá trị kết quả là 0 mod 3. Điều đó có nghĩa là anh ấy muốn hạ cánh chính xác trên một số đồng dạng với 0 modulo 3 tại một trong các lượt của mình. 
3. Mỗi nước đi sẽ thay đổi phần dư theo +1 hoặc -1 modulo 3. Điều này có nghĩa là mọi nước đi chỉ đơn giản là xoay phần dư giữa {0, 1, 2}. 
4. Vì người chơi luân phiên, Vova luôn có thể phản ứng để đẩy phần dư ra khỏi 0 bất cứ khi nào Vanya cố gắng tiếp cận nó trong một bước duy nhất, nhưng anh ta không thể tránh nó vĩnh viễn qua nhiều lần luân phiên bắt buộc. 
5. Trò chơi ngắn, giới hạn ở 10 nước đi. Trong một khoảng thời gian nhỏ như vậy, Vanya có đủ cơ hội một cách hiệu quả để tạo ra một vị trí mà phản hồi của Vova không thể tránh được điểm 0 ở lượt của Vanya trừ khi cấu hình ban đầu đã ở trong chu trình được bảo vệ. 
6. Phân tích kết quả tập trung vào việc kiểm tra xem liệu n % 3 == 0 có giành chiến thắng ngay lập tức hay không hay liệu Vanya có thể bước vào bội số của 3 trong nước đi đầu tiên của mình hay sau một chuỗi bắt buộc ngắn. Trong cách chơi tối ưu, điều này luôn dẫn đến một kết quả xác định chỉ phụ thuộc vào mẫu dư lượng ban đầu và tính chẵn lẻ của nước đi, chứ không phụ thuộc vào độ lớn thực tế của n. 

Trên thực tế, kết quả tối ưu còn đơn giản hóa hơn nữa: Vanya thắng khi và chỉ khi n % 3 != 0. 

### Tại sao nó hoạt động 

Điều bất biến là trạng thái trò chơi giảm hoàn toàn về phần dư modulo 3 và mỗi người chơi chỉ có khả năng xoay phần dư này một bước theo một trong hai hướng. Bởi vì Vanya đi trước và chỉ cần thành công một lần trong nước đi của mình, nên bất kỳ dư lượng khác 0 nào cũng cho phép anh ta chọn hướng đạt 0 modulo 3 ngay lập tức hoặc buộc Vova vào vị trí mà anh ta không thể ngăn cản trong phạm vi giới hạn 10 nước đi. Vì không gian trạng thái có kích thước 3 và cả hai người chơi đều có sức di chuyển đối xứng, nên cấu hình thua ổn định duy nhất đối với Vanya là khi anh ta đã ở mức dư 0 và không thể cải thiện vị trí của mình ở nước đi đầu tiên nếu không cho Vova kiểm soát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n % 3 == 0:
            print("Second")
        else:
            print("First")

if __name__ == "__main__":
    solve()
```Mã đọc từng trường hợp kiểm thử và tính phần dư của n modulo 3. Nếu số đã chia hết cho 3, Vanya bị buộc vào tình thế mà bất kỳ nước đi nào cũng vi phạm điều kiện và Vova có thể duy trì quyền kiểm soát đủ lâu để giới hạn 10 nước đi hết hạn. Nếu không, Vanya có thể ngay lập tức điều chỉnh giá trị ở nước đi đầu tiên để đạt bội số của 3. 

Việc triển khai được cố tình tối giản vì tất cả động lực của trò chơi đều tập trung vào quá trình kiểm tra mô-đun duy nhất này. 

## Ví dụ đã hoạt động 

Xét n = 5. 

| Di chuyển | Người chơi | Giá trị | mod 3 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Bắt đầu | 5 | 2 | Vanya di chuyển | 
| 1 | Vanya | 6 | 0 | Vanya thắng ngay | 

Điều này cho thấy khi bắt đầu từ số dư 2, Vanya có thể chọn +1 và giành chiến thắng ngay lập tức. 

Bây giờ hãy xem xét n = 6. 

| Di chuyển | Người chơi | Giá trị | mod 3 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Bắt đầu | 6 | 0 | Vanya di chuyển | 
| 1 | Vanya | 5 hoặc 7 | 2 hoặc 1 | không thể là 0 | 
| 2 | Vova | điều chỉnh | chu kỳ | trì hoãn | 
| ... | ... | ... | ... | Vova tồn tại đến giới hạn | 

Ở đây Vanya không thể đạt được nước đi thắng ngay lập tức và Vova luôn có thể phản ứng để tránh đưa bội số 3 cho các lượt của Vanya trong vòng 10 nước đi giới hạn, dẫn đến Vanya thua. 

Những ví dụ này thể hiện sự bất đối xứng: chỉ có bội số của 3 mới mang lại sức mạnh cưỡng bức ngay lập tức cho Vanya. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý bằng một thao tác modulo duy nhất | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Các ràng buộc cho phép tối đa 100 trường hợp kiểm thử, do đó, việc kiểm tra thời gian liên tục cho mỗi trường hợp kiểm thử là rất nhanh và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n % 3 == 0:
            out.append("Second")
        else:
            out.append("First")
    return "\n".join(out)

# provided samples
assert run("6\n1\n3\n5\n10\n9\n1000\n") == "First\nSecond\nFirst\nFirst\nSecond\nFirst"

# custom cases
assert run("3\n1\n2\n3\n") == "First\nFirst\nSecond"
assert run("2\n6\n9\n") == "Second\nSecond"
assert run("1\n1000\n") == "First"
assert run("1\n0\n") == "Second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,3 | Thứ nhất, Thứ nhất, Thứ hai | hành vi dư lượng cơ bản | 
| 6,9 | Thứ hai, Thứ hai | bội số của 3 trường hợp thua | 
| 1000 | Đầu tiên | tính nhất quán đầu vào lớn | 
| 0 | Thứ hai | trường hợp chia hết biên | 

## Vỏ cạnh 

Với n = 3, chúng ta bắt đầu với bội số của 3. Nước đi đầu tiên của Vanya phải thay đổi giá trị thành 2 hoặc 4, không chia hết cho 3. Từ thời điểm đó, Vova có thể phản chiếu các nước đi để tránh để Vanya chạm vào bội số của 3 trong lượt của mình và giới hạn 10 nước đi đảm bảo Vanya không thể đột phá. 

Với n = 1, Vanya có thể ngay lập tức chuyển sang 2, số này không chia hết cho 3, nhưng nước đi bổ sung ở lượt tiếp theo cho phép anh ta đạt 3 ở lượt Vanya sau nếu Vova không phản công hoàn hảo. Vì Vanya đi trước và linh hoạt trong việc chọn ±1, nên anh ấy có thể điều khiển chu trình dư lượng về 0 khi tự mình di chuyển trong một số bước nhỏ. 

Với n = 1000, phần dư là 1, nó hoạt động giống hệt với mọi phần dư khác 0 khác. Độ lớn không liên quan, và chỉ riêng chu trình mô-đun đã xác định rằng Vanya có thể giành chiến thắng. 

Những trường hợp này xác nhận rằng chỉ chia cho 3 khi bắt đầu sẽ tạo ra cấu hình thua ổn định cho Vanya khi chơi tối ưu.
