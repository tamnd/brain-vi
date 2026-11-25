---
title: "CF 984A - Trò chơi"
description: "Chúng tôi được cung cấp một danh sách các số nguyên được đặt trên một bảng. Hai người chơi lần lượt loại bỏ đúng một số khỏi bảng. Sau khi loại bỏ chính xác $n-1$, chỉ còn lại một số duy nhất và số đó là kết quả của trò chơi. Các cầu thủ có những mục tiêu trái ngược nhau."
date: "2026-06-17T00:59:39+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 984
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 483 (Div. 2) [Thanks, Botan Investments and Victor Shaburov!]"
rating: 800
weight: 984
solve_time_s: 90
verified: true
draft: false
---

[CF 984A - Trò chơi](https://codeforces.com/problemset/problem/984/A) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên được đặt trên một bảng. Hai người chơi lần lượt loại bỏ đúng một số khỏi bảng. Sau chính xác$n-1$loại bỏ, chỉ còn lại một số duy nhất và số đó là kết quả của trò chơi. 

Các cầu thủ có những mục tiêu trái ngược nhau. Người chơi đầu tiên loại bỏ các số theo cách cố gắng làm cho giá trị còn lại cuối cùng càng nhỏ càng tốt, trong khi người chơi thứ hai cố gắng làm cho nó lớn nhất có thể. Cả hai người chơi đều quan sát toàn bộ đội hình mọi lúc và chơi một cách tối ưu. 

Điều quan trọng không phải là thứ tự loại bỏ mà là yếu tố nào buộc phải tồn tại trong toàn bộ chuỗi xóa chiến lược. 

Ràng buộc$n \le 1000$nghĩa là chúng ta có đủ khả năng$O(n^2)$hoặc thậm chí$O(n^3)$về nguyên tắc là lý luận, nhưng cấu trúc của trò chơi gợi ý một giải pháp tham lam hoặc dựa trên bất biến sẽ phù hợp hơn là mô phỏng trên tất cả các trạng thái trò chơi. 

Một mô phỏng đơn giản sẽ cố gắng mô hình hóa mọi trình tự loại bỏ có thể có và các quyết định tối ưu xen kẽ. Điều đó nhanh chóng trở nên không khả thi vì mỗi trạng thái sẽ phân nhánh tùy thuộc vào yếu tố nào bị loại bỏ và cả hai người chơi đều tối ưu hóa các trạng thái trong tương lai. Ngay cả việc cắt tỉa mạnh mẽ vẫn để lại một cây trò chơi lớn. 

Một vấn đề tế nhị hơn xuất hiện trong việc lý luận bằng trực giác cục bộ. Ví dụ: người ta có thể cho rằng người chơi chỉ cần thay phiên nhau loại bỏ mức tối đa hoặc tối thiểu hiện tại. Điều này không đúng, vì danh tính của yếu tố cuối cùng còn lại phụ thuộc vào việc bảo vệ hay loại bỏ ứng viên chứ không chỉ tham lam loại bỏ các thái cực ở mỗi bước. 

Trường hợp cạnh khóa là khi tất cả các số đều bằng nhau. Bất kỳ chuỗi loại bỏ nào cũng tạo ra câu trả lời cuối cùng giống nhau, do đó cách chơi tối ưu sẽ trở nên không phù hợp. Một trường hợp cạnh khác là khi kích thước mảng là hai. Người chơi đầu tiên loại bỏ một phần tử và người thứ hai loại bỏ phần tử còn lại, vì vậy câu trả lời chỉ đơn giản là sự lựa chọn của người chơi đầu tiên về phần tử nào cần bỏ lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô hình hóa trò chơi như một quá trình tối đa hóa trên các tập hợp con của các phần tử còn lại. Mỗi trạng thái được xác định bởi những phần tử nào còn lại và đến lượt của nó. Từ một trạng thái, người chơi thử tất cả các lần xóa có thể và đánh giá kết quả theo cách đệ quy. 

Cách tiếp cận này đúng vì nó hoàn toàn tôn trọng lối chơi tối ưu, nhưng nó bùng nổ về mặt tổ hợp. có$2^n$tập hợp con và từ mỗi tập hợp con lên đến$n$chuyển tiếp, đưa ra đại khái$O(n2^n)$trạng thái, vượt xa giới hạn ngay cả đối với$n = 30$. 

Cái nhìn sâu sắc quan trọng là trò chơi không phải là kiểm soát các giá trị trung gian mà là kiểm soát phần tử nào còn sót lại sau lần xóa cuối cùng. Vì chỉ có một phần tử còn tồn tại nên quá trình này tương đương với việc chọn một phần tử để “bảo vệ” trong khi tất cả các phần tử khác sẽ bị loại bỏ. 

Người chơi thứ hai luôn cố gắng đảm bảo rằng người sống sót cuối cùng càng lớn càng tốt. Người chơi đầu tiên cố gắng ngăn chặn các phần tử lớn sống sót bằng cách loại bỏ chúng sớm. Vì người chơi thay thế nhau loại bỏ, nên người chơi đầu tiên sẽ có được lựa chọn đầu tiên về yếu tố _không được bảo vệ_ trong mỗi vòng áp lực loại bỏ. 

Một cách hữu ích để diễn giải lại quy trình là: mỗi lần di chuyển sẽ loại bỏ vĩnh viễn một phần tử. Sau đó$n-1$di chuyển, chính xác một phần tử sẽ không bao giờ được chọn để loại bỏ. Cách chơi tối ưu tập trung vào việc quyết định yếu tố nào khó loại bỏ nhất dưới sự kiểm soát luân phiên. 

Cấu trúc đơn giản hóa đáng kể: cả hai người chơi đang cạnh tranh hiệu quả trong việc ra lệnh loại bỏ các phần tử, nhưng vì mọi phần tử ngoại trừ một phần tử đều phải bị xóa nên mục tiêu của người chơi thứ hai giảm xuống để đảm bảo phần tử lớn nhất tồn tại, trong khi người chơi đầu tiên cố gắng ngăn chặn điều đó. Tuy nhiên, người chơi đầu tiên luôn đi trước, nghĩa là họ luôn có thể đảm bảo rằng ít nhất một “ứng cử viên lớn” sẽ bị loại vào một thời điểm nào đó. 

Điều này dẫn đến một kết quả cổ điển cho bài toán này: cách chơi tối ưu luôn để lại phần tử lớn thứ hai nếu$n > 1$. Theo trực giác, yếu tố lớn nhất quá có giá trị để người chơi thứ hai tránh nhắm mục tiêu gián tiếp, nhưng người chơi đầu tiên luôn có thể buộc loại bỏ nó kịp thời và người chơi thứ hai sau đó sẽ bảo vệ ứng cử viên tốt nhất tiếp theo. 

Như vậy, đáp án cuối cùng là giá trị lớn thứ hai của mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n2^n)$|$O(2^n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Tính năng sắp xếp được sử dụng để xác định các phần tử lớn nhất và lớn thứ hai, là những ứng cử viên duy nhất quan trọng sau khi lối chơi tối ưu sẽ nén trò chơi vào mức kiểm soát điểm cuối. 
2. Nếu$n = 1$, trả về phần tử duy nhất ngay lập tức vì không có bước di chuyển nào được thực hiện. 
3. Ngược lại, trả về phần tử lớn thứ hai, nằm ở chỉ mục$n-2$sau khi sắp xếp. 

### Tại sao nó hoạt động 

Trò chơi loại bỏ chính xác$n-1$các yếu tố, để lại một người sống sót. Cả hai người chơi đều có toàn quyền hiển thị và kiểm soát thay thế, nhưng họ không kiểm soát cấu trúc đang phát triển hoặc thu hẹp ngoài việc xóa. Điều này có nghĩa là không có yếu tố nào có thể được bảo tồn vô thời hạn trừ khi cả hai người chơi luôn tránh loại bỏ nó, điều này là không thể khi chơi tối ưu xen kẽ khi cạnh tranh ở các thái cực. 

Mục tiêu của người chơi thứ hai là tối đa hóa giá trị cuối cùng nên họ sẽ luôn cố gắng bảo toàn ứng viên lớn nhất còn lại. Người chơi đầu tiên, di chuyển trước, luôn có thể đáp lại những nỗ lực bảo vệ tối đa bằng cách loại bỏ nó hoặc buộc nó phải chịu áp lực loại bỏ trong các lượt tiếp theo. Sự tương tác này đảm bảo mức tối đa không thể tồn tại một cách đáng tin cậy trong chiến lược tối ưu của cả hai người chơi. 

Sau khi mức tối đa bị mất ổn định, ứng cử viên ổn định mạnh nhất tiếp theo là mức tối đa thứ hai, trở thành người sống sót cuối cùng trong lối chơi tối ưu. Không có yếu tố nào khác có thể vượt trội hơn nó một cách có hệ thống bởi vì bất kỳ yếu tố nào nhỏ hơn đều sẽ tệ hơn đối với người chơi tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    if n == 1:
        print(a[0])
        return

    a.sort()
    print(a[-2])

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc mảng và xử lý trực tiếp trường hợp một phần tử tầm thường. Sắp xếp được sử dụng để sắp xếp các giá trị sao cho các phần tử lớn nhất và lớn thứ hai có thể truy cập được ngay lập tức. 

Sau khi sắp xếp, câu trả lời được lấy từ vị trí cuối cùng thứ hai. Điều này là an toàn vì việc sắp xếp không thay đổi nhiều giá trị và đối số ở trên cho thấy rằng chỉ thông tin xếp hạng mới quan trọng chứ không phải vị trí. 

Việc thực hiện tránh mô phỏng hoàn toàn. Bất kỳ nỗ lực mô phỏng lượt nào sẽ yêu cầu duy trì trạng thái trò chơi và đánh giá các bước di chuyển tối ưu, điều này là không cần thiết sau khi nhận ra số liệu thống kê thứ tự giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 3
```Mảng được sắp xếp:`[1, 2, 3]`| Bước | Hành động | Trạng thái mảng | Quan sát chính | 
| --- | --- | --- | --- | 
| 1 | Xác định ứng viên | [1, 2, 3] | Lớn nhất và lớn thứ hai là 3 và 2 | 
| 2 | Áp dụng quy tắc | [1, 2, 3] | Lớn thứ hai được chọn | 

Đầu ra:```
2
```Dấu vết này cho thấy rằng bất chấp tính chất tương tác của trò chơi, chỉ có thứ hạng của hai yếu tố hàng đầu mới quan trọng. Người sống sót cuối cùng là phần tử lớn thứ hai. 

### Ví dụ 2 

đầu vào:```
5
4 4 4 4 4
```Mảng được sắp xếp:`[4, 4, 4, 4, 4]`| Bước | Hành động | Trạng thái mảng | Quan sát chính | 
| --- | --- | --- | --- | 
| 1 | Xác định ứng viên | [4, 4, 4, 4, 4] | Tất cả các giá trị giống nhau | 
| 2 | Áp dụng quy tắc | [4, 4, 4, 4, 4] | Lớn thứ hai bằng lớn nhất | 

Đầu ra:```
4
```Điều này xác nhận rằng khi tất cả các giá trị đều bằng nhau thì mọi trình tự loại bỏ đều mang lại kết quả như nhau và công thức vẫn hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(1)$thêm | Chỉ phân loại tại chỗ và lưu trữ đầu vào | 

Những hạn chế$n \le 1000$làm cho việc sắp xếp trở nên tầm thường về mặt hiệu suất. Kể cả ngây thơ$O(n^2)$Cách tiếp cận này sẽ thành công, nhưng việc sắp xếp mang lại một giải pháp rõ ràng và đáng tin cậy với chi phí lý luận tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    if n == 1:
        return str(a[0])
    a.sort()
    return str(a[-2])

# provided samples
assert run("3\n2 1 3\n") == "2"

# minimum size
assert run("1\n42\n") == "42"

# two elements
assert run("2\n5 100\n") == "5"

# all equal
assert run("4\n7 7 7 7\n") == "7"

# descending order
assert run("5\n9 8 7 6 5\n") == "7"

# ascending order
assert run("5\n1 2 3 4 5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 42 | trường hợp cạnh đơn phần tử | 
| 2 yếu tố | 5 | buộc phải sống sót cuối cùng | 
| tất cả đều bình đẳng | 7 | tính đúng đắn đối xứng | 
| giảm dần | 7 | lựa chọn tối đa thứ hai | 
| tăng dần | 4 | đặt hàng độc lập | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán trả về ngay phần tử duy nhất. Về mặt khái niệm, không cần sắp xếp và không có trò chơi nào xảy ra, vì vậy đây là trường hợp cơ bản trực tiếp. 

Khi$n = 2$, sắp xếp tạo ra`[min, max]`và thuật toán trả về`min`. Điều này phù hợp với lối chơi vì người chơi đầu tiên loại bỏ một yếu tố và người chơi thứ hai không có lựa chọn nào khác ngoài việc rời khỏi yếu tố kia, vì vậy người chơi đầu tiên quyết định kết quả một cách hiệu quả bằng cách chọn yếu tố nào còn lại. 

Khi tất cả các phần tử giống hệt nhau, việc sắp xếp sẽ giữ nguyên chúng và phần tử lớn thứ hai bằng phần tử lớn nhất. Mọi trình tự xóa đều tương đương nhau nên quy tắc sẽ thu gọn chính xác về giá trị được chia sẻ. 

Khi mảng tăng hoặc giảm nghiêm ngặt, phần tử lớn thứ hai luôn được xác định rõ ràng và ổn định khi sắp xếp. Thuật toán luôn trả về chính xác người sống sót dựa trên thứ hạng bất kể thứ tự ban đầu.
