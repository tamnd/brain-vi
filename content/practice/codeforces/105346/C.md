---
title: "CF 105346C - Hành Lang Ma Quái"
description: "Chúng ta được cung cấp một chuỗi nhị phân biểu thị một dãy đèn lồng, trong đó mỗi vị trí được thắp sáng hoặc không sáng. Trong một lần di chuyển, chúng ta được phép chọn bất kỳ đoạn liền kề nào và lật từng bit bên trong nó, biến số 0 thành số 1 và số 1 thành số 0."
date: "2026-06-23T05:43:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 88
verified: false
draft: false
---

[CF 105346C - Hành lang ma quái](https://codeforces.com/problemset/problem/105346/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân biểu thị một dãy đèn lồng, trong đó mỗi vị trí được thắp sáng hoặc không sáng. Trong một lần di chuyển, chúng ta được phép chọn bất kỳ đoạn liền kề nào và lật từng bit bên trong nó, biến số 0 thành số 1 và số 1 thành số 0. Mục tiêu là đạt được một cấu hình trong đó toàn bộ chuỗi trở nên đồng nhất, tất cả các số 0 hoặc tất cả các số 1, sử dụng càng ít lần lật phân đoạn như vậy càng tốt. 

Kích thước đầu vào có thể lên tới một trăm nghìn ký tự. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân đoạn có thể có hoặc mô phỏng chuỗi các lần lật một cách rõ ràng. Bất cứ điều gì tệ hơn thời gian tuyến tính hoặc gần tuyến tính sẽ gặp khó khăn, vì chỉ riêng số lượng chuỗi con là bậc hai. 

Một ý tưởng ngây thơ là thử chọn mọi trạng thái mục tiêu cuối cùng có thể có, tất cả số 0 hoặc tất cả số 1, sau đó mô phỏng các phân đoạn lật một cách tham lam hoặc ngây thơ cho đến khi chuỗi khớp. Một nỗ lực không chính xác phổ biến khác là lật từng khối ký tự không khớp tối đa một cách độc lập mà không xem xét các cơ hội hợp nhất, điều này có thể làm quá số phép toán. 

Ví dụ, hãy xem xét`010`. Nếu chúng ta nhắm đến tất cả các số 0, việc lật đoạn giữa sẽ mang lại`000`trong một lần di chuyển. Một kẻ tham lam lật từng ký tự không khớp riêng lẻ sẽ gợi ý không chính xác hai thao tác. Ngược lại, trên`01010`, các quyết định cục bộ bất cẩn có thể dễ dàng tính quá mức hoặc quá thấp vì việc đảo ngược một phân đoạn sẽ làm thay đổi nhiều ranh giới trong tương lai. 

Vấn đề tinh tế là một lần lật không hoạt động cục bộ một cách cô lập, nó thay đổi cấu trúc liền kề, do đó chi phí thực sự phụ thuộc vào sự chuyển đổi giữa các ký tự liên tiếp chứ không phải bản thân các ký tự riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét mọi chuỗi chuyển đoạn có thể xảy ra. Ngay cả khi chúng ta hạn chế chỉ lật các đoạn giúp di chuyển về phía mục tiêu, số cách để chọn các đoạn vẫn theo cấp số nhân vì sau mỗi lần lật, chuỗi sẽ thay đổi và tạo ra các đoạn mới khả thi. Điều này nhanh chóng trở nên không khả thi khi vượt quá n rất nhỏ. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cố gắng sửa một chuỗi mục tiêu, tất cả các số 0 hoặc tất cả các số 1, sau đó mô phỏng một sự điều chỉnh tham lam. Nếu chúng ta quét từ trái sang phải, bất cứ khi nào chúng ta gặp sự không khớp với mục tiêu, chúng ta sẽ chuyển từ vị trí đó về phía trước cho đến khi giải quyết được sự không khớp. Điều này đúng đối với một mục tiêu cố định, nhưng vẫn đòi hỏi phải suy luận về số lần phải lật. 

Quan sát quan trọng là khi chúng tôi cố định mục tiêu, chiến lược tối ưu hoàn toàn được xác định bằng các chuyển đổi trong chuỗi. Giả sử chúng ta nhắm mục tiêu tất cả số không. Mỗi khi chúng ta thấy một sự chuyển đổi`0 → 1`, điều đó có nghĩa là chúng ta đang bước vào một phân đoạn mà cuối cùng phải được đảo ngược để sửa những phân đoạn đó. Tương tự, mỗi lần chuyển đổi`1 → 0`đánh dấu sự kết thúc của một phân đoạn như vậy. Mỗi khối liên tục phải được lật chính xác một lần và mỗi lần lật có thể bao phủ toàn bộ khối một cách tối ưu. 

Do đó, số lần lật cần thiết để chuyển chuỗi thành tất cả các số 0 bằng số khối liền kề của chuỗi số 1. Một cách đối xứng, số lần lật cần thiết để chuyển đổi thành tất cả số 1 bằng số khối số 0 liền kề. Câu trả lời đơn giản là giá trị tối thiểu của hai giá trị này. 

Vấn đề giảm từ việc chọn phân đoạn động sang đếm số lần chạy trong chuỗi nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi một lần và đếm xem có bao nhiêu đoạn liền kề nhau`'1'`hiện hữu. Một phân đoạn mới bắt đầu bất cứ khi nào`'1'`xuất hiện và ký tự trước đó không`'1'`. Điều này đếm xem có bao nhiêu nhóm riêng biệt phải được xử lý nếu chúng ta muốn biến mọi thứ thành số không. 
2. Quét chuỗi một lần và đếm xem có bao nhiêu đoạn liền kề nhau`'0'`tồn tại bằng cách sử dụng cùng một logic. Điều này thể hiện số lần lật cần thiết nếu chúng ta muốn biến mọi thứ thành một. 
3. So sánh hai số đếm và xuất ra giá trị nhỏ hơn. Điều này tương ứng với việc chọn trạng thái đồng nhất cuối cùng rẻ hơn. 

Tại sao điều này hiệu quả: mỗi lần lật trong một giải pháp tối ưu có thể được giả định là bao phủ toàn bộ đoạn liền kề tối đa gồm các ký tự giống hệt nhau khác với mục tiêu. Việc chia một phân đoạn thành nhiều lần lật không bao giờ có ích, vì việc lật một vùng liền kề lớn hơn không làm tăng chi phí nhưng lại giảm phân mảnh. Do đó, mỗi khối tối đa đóng góp chính xác một thao tác cần thiết và không có tương tác nào giữa các khối riêng biệt có thể làm giảm số lượng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_blocks(s, ch):
    cnt = 0
    n = len(s)
    i = 0
    while i < n:
        if s[i] == ch:
            cnt += 1
            while i < n and s[i] == ch:
                i += 1
        else:
            i += 1
    return cnt

def solve():
    n = int(input().strip())
    s = input().strip()

    ones = count_blocks(s, '1')
    zeros = count_blocks(s, '0')

    print(min(ones, zeros))

if __name__ == "__main__":
    solve()
```chức năng`count_blocks`tính số lần chạy liền kề của một ký tự nhất định. Nó tiến qua chuỗi và chỉ tăng bộ đếm khi gặp phần đầu của một khối mới, bỏ qua phần còn lại của khối đó ngay lập tức. Điều này đảm bảo truyền tải tuyến tính. 

Chúng tôi tính toán cả số lượng khối một và khối không vì chúng tương ứng với hai cấu hình mục tiêu có thể có. Câu trả lời cuối cùng là tối thiểu, vì chúng ta có thể tự do lựa chọn trạng thái thống nhất mà chúng ta muốn đạt được. 

Một lỗi phổ biến là cố gắng mô phỏng các cú lật một cách rõ ràng. Điều đó là không cần thiết vì các lần lật không bao giờ cần chồng chéo trong một giải pháp tối ưu và cấu trúc của bài toán đảm bảo rằng các ranh giới khối sẽ xác định đầy đủ chi phí. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:`101001011001`Chúng tôi tính toán các khối số một và số không. 

| chỉ mục | char | khối '1' mới? | những khối | khối số không | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | vâng | 1 | 0 | 
| 1 | 0 | - | 1 | 1 | 
| 2 | 1 | vâng | 2 | 1 | 
| 3 | 0 | - | 2 | 2 | 
| 4 | 0 | - | 2 | 2 | 
| 5 | 1 | vâng | 3 | 2 | 
| 6 | 0 | - | 3 | 3 | 
| 7 | 1 | vâng | 4 | 3 | 
| 8 | 1 | - | 4 | 3 | 
| 9 | 0 | - | 4 | 4 | 
| 10 | 0 | - | 4 | 4 | 
| 11 | 1 | vâng | 5 | 4 | 

Số cuối cùng là số 1 = 5 và số 0 = 4, vì vậy câu trả lời là 4. 

Dấu vết này cho thấy giải pháp hoàn toàn là theo dõi các chuyển tiếp và không phụ thuộc vào mô phỏng lật thực tế. Mỗi lần chúng tôi nhập lại một chuỗi ký tự giống hệt nhau, chúng tôi chỉ tăng một lần. 

Một ví dụ thứ hai: 

đầu vào:`000111000`| chỉ mục | char | những khối | khối số không | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 1 | 
| 3 | 1 | 1 | 1 | 
| 4 | 1 | 1 | 1 | 
| 5 | 1 | 1 | 1 | 
| 6 | 0 | 1 | 2 | 
| 7 | 0 | 1 | 2 | 
| 8 | 0 | 1 | 2 | 

Câu trả lời là min(1, 2) = 1. 

Điều này xác nhận rằng một cú lật có thể khắc phục được một khối ở giữa liền kề khi chọn mục tiêu thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập nhiều nhất một lần trong mỗi lần quét | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các giới hạn vì nó thực hiện một số lần tuyến tính không đổi trên chuỗi, không yêu cầu bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def count_blocks(s, ch):
        cnt = 0
        i = 0
        n = len(s)
        while i < n:
            if s[i] == ch:
                cnt += 1
                while i < n and s[i] == ch:
                    i += 1
            else:
                i += 1
        return cnt

    n = int(input().strip())
    s = input().strip()
    return str(min(count_blocks(s, '1'), count_blocks(s, '0')))

# provided sample
assert run("12\n101001011001\n") == "4"

# all equal ones
assert run("5\n11111\n") == "0"

# all equal zeros
assert run("5\n00000\n") == "0"

# alternating
assert run("4\n0101\n") == "2"

# single character
assert run("1\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 0 | đã đồng nhất không cần lật | 
| tất cả số không | 0 | trường hợp cơ sở đối xứng | 
| xen kẽ | 2 | phân mảnh trong trường hợp xấu nhất | 
| char đơn | 0 | xử lý ranh giới tối thiểu | 

## Vỏ cạnh 

Một chuỗi như`11111`không có chuyển tiếp nội bộ. Thuật toán đếm một khối số 1 và 0 khối số 0 là 0. Mức tối thiểu bằng 0, phù hợp với thực tế là không cần lật. 

Vì`010101`, mỗi ký tự thay thế nhau, tạo ra ba khối một và ba khối không. Thuật toán trả về chính xác ba, phản ánh rằng mỗi ký tự bị cô lập yêu cầu sự điều chỉnh riêng theo bất kỳ lựa chọn mục tiêu nào. 

Đối với một mô hình như`000111000`, đoạn ở giữa là đoạn duy nhất khác với mục tiêu đã chọn là tất cả các số 0. Thuật toán đếm một khối duy nhất, nắm bắt chính xác rằng một lần lật có thể bao phủ toàn bộ khu vực ở giữa trong một thao tác.
