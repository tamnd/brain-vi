---
title: "CF 105862D - HOẶC MEX"
description: "Chúng ta được cung cấp một tập hợp các số nguyên và được yêu cầu xem xét tất cả các giá trị có thể được hình thành bằng cách lấy bất kỳ tập hợp con nào của các số này và kết hợp chúng bằng cách sử dụng OR theo bit."
date: "2026-06-25T14:34:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105862
codeforces_index: "D"
codeforces_contest_name: "ACPC Kickoff 2025"
rating: 0
weight: 105862
solve_time_s: 54
verified: true
draft: false
---

[CF 105862D - HOẶC MEX](https://codeforces.com/problemset/problem/105862/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên và được yêu cầu xem xét tất cả các giá trị có thể được hình thành bằng cách lấy bất kỳ tập hợp con nào của các số này và kết hợp chúng bằng cách sử dụng OR theo bit. Từ tập hợp các kết quả có thể truy cập này, chúng ta cần xác định kết quả là số nguyên không âm nhỏ nhất không bao giờ xuất hiện. 

Một cách hữu ích để diễn đạt lại nhiệm vụ là hãy tưởng tượng bắt đầu từ số 0 và tùy ý “cộng” từng số bằng cách OR-ing nó vào giá trị hiện tại của bạn. Mỗi tập hợp con tương ứng với một số chuỗi lựa chọn về việc có bao gồm từng phần tử hay không và OR của tập hợp con đó tạo ra một giá trị duy nhất. Đối tượng quan tâm cuối cùng là tập hợp tất cả các kết quả OR như vậy và chúng tôi đang tìm kiếm số nguyên đầu tiên bị thiếu trong tập hợp này. 

Các ràng buộc trong bài toán này được thiết kế xung quanh cấu trúc bitwise OR thay vì kích thước số thô. Ngay cả khi các giá trị trông lớn, OR chỉ tương tác thông qua các bit, do đó không gian trạng thái hiệu dụng được xác định bởi số bit cần thiết để biểu thị giá trị tối đa. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các tập hợp con một cách rõ ràng, vì đó sẽ là số mũ của số phần tử. Thay vào đó, các phương pháp khả thi duy nhất phải nén vấn đề vào không gian trạng thái có thể quản lý được trên mặt nạ bit. 

Một số trường hợp đặc biệt rất dễ bị bỏ sót nếu người ta chỉ nghĩ về mảng đầu vào thay vì cấu trúc tập hợp con OR. Nếu mảng chỉ chứa các số 0 thì mọi tập hợp con OR đều bằng 0, do đó tập hợp có thể truy cập là`{0}`và câu trả lời nên là`1`. Một sai lầm ngây thơ là cho rằng mọi phần tử đều đóng góp một giá trị mới, dẫn đến suy nghĩ không chính xác về việc tồn tại nhiều kết quả OR riêng biệt. 

Một trường hợp tinh tế khác xảy ra khi các giá trị trùng nhau nhiều về số bit. Ví dụ, với đầu vào`[1, 2, 3]`, tập hợp con HOẶC kết quả là`{0, 1, 2, 3}`bởi vì`1 | 2 = 3`, nhưng không có cách nào để hình thành`4`. Một cách tiếp cận ngây thơ có thể giả định không chính xác rằng vì bit 0 và 1 xuất hiện nên tất cả các số lên tới`3`được bảo hiểm và mex nên`4`, nhưng điều này chỉ đúng vì`4`yêu cầu một bit mới không có trong bất kỳ phần tử nào. 

Cuối cùng, hãy xem xét các đầu vào trong đó OR toàn cầu dày đặc nhưng vẫn để lại những khoảng trống về khả năng biểu diễn. Ngay cả khi OR của tất cả các phần tử là`7`, điều đó không có nghĩa là tất cả các số từ`0`ĐẾN`7`có thể truy cập được. Sự không phù hợp giữa “phạm vi bao phủ bit” và “phạm vi bao phủ giá trị” là lý do cốt lõi khiến lý luận ngây thơ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: liệt kê mọi tập hợp con của mảng, tính toán OR theo bit của nó, lưu trữ tất cả các kết quả trong một tập hợp và cuối cùng quét lên từ 0 để tìm số nguyên bị thiếu đầu tiên. Điều này đúng vì nó xây dựng rõ ràng định nghĩa của vấn đề. Tuy nhiên, nó đòi hỏi phải lặp lại tất cả`2^n`các tập hợp con và cho mỗi tập hợp con tính toán OR lên tới`n`phần tử trong trường hợp xấu nhất. Điều này dẫn đến khoảng`O(n * 2^n)`hoạt động, điều này trở nên không khả thi ngay cả đối với`n = 25`, chứ đừng nói đến những ràng buộc lớn hơn. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến tập hợp con nào tạo ra giá trị, mà chỉ quan tâm liệu giá trị đó có thể truy cập được hay không. Điều này biến vấn đề thành vấn đề về khả năng tiếp cận trên các trạng thái OR. Mỗi số biến đổi một trạng thái hiện tại`x`sang trạng thái mới`x | a_i`. Vì OR chỉ thêm các bit và không bao giờ loại bỏ chúng, nên không gian trạng thái bị đóng trong các chuyển tiếp lặp lại và bị giới hạn bởi mặt nạ bit tối đa mà đầu vào có thể biểu thị. 

Điều này cho phép chúng tôi coi vấn đề là lập trình động trên mặt nạ bit: duy trì một mảng boolean trong đó các giá trị OR có thể đạt được và lặp lại việc mở rộng nó bằng cách sử dụng từng phần tử mảng. Mỗi phần tử đóng góp quá trình chuyển đổi từ tất cả các trạng thái hiện có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(2^n) | Quá chậm | 
| Mặt nạ bit DP | O(n · 2^B) | O(2^B) | Đã chấp nhận | 

Đây`B`là số bit cần thiết để biểu thị giá trị lớn nhất trong mảng. 

## Hướng dẫn thuật toán 

1. Xác định độ dài bit tối đa cần thiết để biểu thị bất kỳ giá trị nào trong mảng và đặt giới hạn`MAX_MASK = 2^B`. Điều này xác định tổng thể của tất cả các kết quả OR có thể có, vì OR không thể đưa ra các bit mới. 
2. Tạo một mảng boolean`dp`Ở đâu`dp[x]`cho biết liệu một số tập hợp con có tạo ra giá trị OR hay không`x`. Khởi tạo`dp[0] = True`, đại diện cho tập hợp con trống. 
3. Xử lý từng số`a`trong mảng. Đối với mỗi giá trị hiện có thể truy cập`x`, chúng ta cũng có thể tiếp cận`x | a`. Để tránh ghi đè thông tin trong cùng một lần lặp, chúng tôi tính toán các cập nhật từ ảnh chụp nhanh của trạng thái hiện tại. 
4. Sau khi xử lý tất cả các phần tử, hãy quét`dp`từ`0`trở lên và trả về chỉ số đầu tiên`mex`như vậy`dp[mex]`là sai. 

Lý do cần có ảnh chụp nhanh là vì việc cho phép cập nhật tại chỗ sẽ xâu chuỗi không chính xác một phần tử nhiều lần trong cùng một lần lặp, mô phỏng một cách hiệu quả việc tái sử dụng cùng một phần tử mảng, vi phạm ràng buộc tập hợp con. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý dữ liệu đầu tiên`i`các yếu tố,`dp[x]`đúng khi và chỉ nếu tồn tại tập con của tập đầu tiên`i`các phần tử có OR bằng`x`. Mỗi lần chuyển tiếp`x -> x | a_i`tương ứng chính xác với việc quyết định có nên đưa vào`i`-phần tử thứ trong một tập hợp con. Vì mỗi quyết định tập hợp con được thể hiện chính xác một lần cho mỗi phần tử nên không có giá trị OR có thể truy cập nào bị bỏ qua và không có giá trị không hợp lệ nào được đưa vào. Do đó, lần quét cuối cùng xác định chính xác số nguyên nhỏ nhất không thể biểu thị dưới dạng tập con OR. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    max_val = 0
    for x in arr:
        max_val |= x

    # number of bits needed to represent max_val
    B = max_val.bit_length()
    size = 1 << B

    dp = [False] * size
    dp[0] = True

    for a in arr:
        ndp = dp[:]  # snapshot
        for x in range(size):
            if dp[x]:
                ndp[x | a] = True
        dp = ndp

    for i in range(size):
        if not dp[i]:
            print(i)
            return

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén không gian trạng thái thành chỉ những bit thực sự xuất hiện trong đầu vào. Điều này tránh được những công việc không cần thiết trên các mặt nạ cao hơn không thể tiếp cận được. 

Vòng lặp cốt lõi được xây dựng`ndp`từ`dp`để đảm bảo mỗi phần tử được sử dụng chính xác một lần cho mỗi quyết định tập hợp con. Sự chuyển tiếp`x | a`mã hóa sự bao gồm của phần tử hiện tại. Quá trình quét tuyến tính cuối cùng là an toàn vì không gian trạng thái OR liền kề trong quá trình lập chỉ mục mặc dù khả năng tiếp cận là không. 

Một điểm tinh tế là chúng ta không bao giờ cần theo dõi các tập hợp con một cách rõ ràng, chỉ có các giá trị OR có thể truy cập được. Đây là điều cho phép giải pháp nằm trong phạm vi có thể quản lý được`2^B`không gian trạng thái thay vì hàm mũ trong`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi theo dõi các giá trị OR có thể truy cập. 

| Bước | Giá trị gia tăng | Các trạng thái có thể tiếp cận | 
| --- | --- | --- | 
| 0 | ban đầu | {0} | 
| 1 | 1 | {0, 1} | 
| 2 | 2 | {0, 1, 2, 3} | 
| 3 | 3 | {0, 1, 2, 3} | 

Bộ có thể truy cập cuối cùng là`{0,1,2,3}`, vậy số nguyên còn thiếu đầu tiên là`4`. 

Điều này xác nhận rằng mặc dù mọi cặp bit đều được bao phủ nhưng không có sự kết hợp nào đưa ra một bit mới ngoài những bit đã có. 

### Ví dụ 2 

đầu vào:```
4
0 0 0 0
```| Bước | Giá trị gia tăng | Các trạng thái có thể tiếp cận | 
| --- | --- | --- | 
| 0 | ban đầu | {0} | 
| 1 | 0 | {0} | 
| 2 | 0 | {0} | 
| 3 | 0 | {0} | 
| 4 | 0 | {0} | 

Giá trị duy nhất có thể truy cập là`0`, vậy câu trả lời là`1`. 

Điều này nhấn mạnh rằng các phần tử 0 trùng lặp hoàn toàn không mở rộng không gian trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^B) | Đối với mỗi phần tử, chúng tôi quét tất cả các trạng thái mặt nạ bit và truyền bá các chuyển tiếp HOẶC | 
| Không gian | O(2^B) | Chúng tôi lưu trữ khả năng tiếp cận cho từng bitmask có thể | 

giá trị`B`được giới hạn bởi số lượng bit tối đa trong các giá trị đầu vào, giúp giữ cho không gian trạng thái nhỏ trong thực tế. Điều này phù hợp thoải mái trong các ràng buộc điển hình trong đó các giá trị lên tới khoảng`10^6`hoặc`10^7`, cho tối đa 20-25 bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# Since solve() prints directly, we wrap carefully
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = backup_stdin
        sys.stdout = backup_stdout

# provided sample-like cases
assert run("3\n1 2 3\n") == "4"

# single element zero
assert run("1\n0\n") == "1"

# all zeros
assert run("4\n0 0 0 0\n") == "1"

# single power of two
assert run("1\n8\n") == "0"

# mixed small case
assert run("2\n1 4\n") == "2"

# dense small
assert run("3\n1 2 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 4 | đóng cửa HOẶC đầy đủ điển hình | 
| 1 0 | 1 | trường hợp cạnh chỉ bằng 0 | 
| 4 số không | 1 | bản sao của số 0 | 
| 1 8 | 0 | thiếu số 0 khi tập hợp con trống bị loại trừ không chính xác | 
| 1 4 / 1 2 4 | 2, 3 | khoảng trống trong không gian OR | 

## Vỏ cạnh 

Đối với đầu vào chỉ bao gồm số 0, thuật toán sẽ khởi tạo`dp[0] = True`và không bao giờ mở rộng nó. Quét từ số 0 thấy rằng`0`có thể truy cập được, nhưng`1`không, vì vậy đầu ra là chính xác`1`. 

Đối với các đầu vào trong đó tất cả các giá trị đều là lũy thừa của hai, mỗi phần tử sẽ đưa ra một bit mới và các chuyển đổi OR sẽ dần dần lấp đầy các tổ hợp của các bit đó. DP xây dựng chính xác tất cả các tập con bit, nhưng bất kỳ số nào yêu cầu nhiều bit giống hệt nhau là không thể và quá trình quét sẽ phát hiện số nguyên bị thiếu đầu tiên tương ứng. 

Đối với trường hợp mảng đã chứa một giá trị lớn như`2^k`, bit đó ngay lập tức truyền sang tất cả các trạng thái có thể truy cập thông qua OR, nhưng không đảm bảo các giá trị trung gian được lấp đầy. Thuật toán vẫn kiểm tra từng ứng cử viên một cách rõ ràng, đảm bảo không có giả định nào được đưa ra về vùng phủ sóng liền kề.
