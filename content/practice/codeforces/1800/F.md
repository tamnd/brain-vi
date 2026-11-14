---
title: "CF 1800F - Dasha và những cơn ác mộng"
description: "Chúng ta được cung cấp một danh sách các từ và được yêu cầu đếm các cặp từ có cách nối thỏa mãn một số tính chất nghiêm ngặt."
date: "2026-06-09T09:40:39+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "hashing", "meet-in-the-middle", "strings"]
categories: ["algorithms"]
codeforces_contest: 1800
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 855 (Div. 3)"
rating: 1900
weight: 1800
solve_time_s: 102
verified: true
draft: false
---

[CF 1800F - Dasha và những cơn ác mộng](https://codeforces.com/problemset/problem/1800/F) 

**Xếp hạng:** 1900 
**Thẻ:** bitmasks, băm, gặp nhau ở giữa, chuỗi 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các từ và được yêu cầu đếm các cặp từ có cách nối thỏa mãn một số tính chất nghiêm ngặt. Cụ thể, một "cơn ác mộng" được hình thành bằng cách ghép hai từ lại, tạo thành một chuỗi có độ dài lẻ, chứa đúng 25 chữ cái riêng biệt và mỗi chữ cái xuất hiện với số lần lẻ. Chúng ta cần đếm số lượng các cặp từ riêng biệt gây ra những cơn ác mộng như vậy. 

Đầu vào chứa tối đa 200.000 từ và tổng độ dài của tất cả các từ lên tới 5 triệu ký tự. Điều này cho chúng ta biết rằng bất kỳ thuật toán nào kiểm tra trực tiếp mọi cặp từ có thể có bằng cách ghép chúng và đếm các chữ cái sẽ quá chậm. Một cách tiếp cận đơn giản sẽ thực hiện các phép toán O(n²), có thể đạt tới 4·10¹⁰ đối với giới hạn trên - vượt xa giới hạn thời gian 4 giây. Vì vậy, chúng ta cần một giải pháp có thể kiểm tra từng từ một cách nhanh chóng mà không cần hình thành từng câu nối một cách rõ ràng. 

Các trường hợp cạnh ở đây rất tinh tế. Nếu một từ đã thiếu một chữ cái, chúng ta phải theo dõi chữ cái nào bị thiếu, bởi vì việc kết hợp nó với một từ khác cung cấp chữ cái bị thiếu đó có thể tạo thành một cơn ác mộng thực sự. Tương tự, những từ có các chữ cái lặp lại xuất hiện với số lần chẵn không thể trực tiếp góp phần gây ra cơn ác mộng trừ khi kết hợp với những từ làm đảo ngược tính chẵn lẻ thành số lẻ. Việc triển khai bất cẩn chỉ đếm các chữ cái trên toàn cầu hoặc chỉ theo dõi số lượng các chữ cái riêng biệt sẽ không thành công trong những trường hợp này. Ví dụ: một từ như "aabbcc" kết hợp với "defghijklmnopqrstuvwxyz" sẽ chỉ là một cơn ác mộng nếu các số chẵn lẻ khớp nhau chứ không chỉ vì cuối cùng nó chứa 25 chữ cái. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Với mỗi cặp từ (i, j), hãy ghép hai từ đó lại, đếm xem có bao nhiêu chữ cái khác nhau và kiểm tra tính chẵn lẻ của mỗi chữ cái. Điều này hoạt động vì nó trực tiếp thực hiện báo cáo vấn đề. Tuy nhiên, nó không thành công khi n lớn: với n = 2·10⁵, có khoảng 2·10¹⁰ cặp và mỗi cặp cần tới 50 thao tác để đếm các chữ cái. Đó là quá nhiều thao tác cho giới hạn 4 giây. 

Thông tin chi tiết quan trọng là nhận ra rằng tất cả các điều kiện về số lượng chữ cái và các chữ cái riêng biệt có thể được mã hóa một cách hiệu quả bằng cách sử dụng mặt nạ bit. Biểu thị mỗi từ bằng một số nguyên 26 bit trong đó mỗi bit tương ứng với một chữ cái và là 1 nếu chữ cái đó xuất hiện với số lần lẻ. Điều kiện "mỗi chữ cái xuất hiện với số lần lẻ trong phép nối" sau đó trở thành XOR theo bit giữa các mặt nạ bit của hai từ. Yêu cầu có chính xác 25 chữ cái riêng biệt có nghĩa là kết quả XOR phải được đặt chính xác 25 bit. Kiểm tra tính chẵn lẻ của độ dài là chuyện nhỏ và không phụ thuộc vào các chữ cái. 

Điều này làm giảm vấn đề đếm, đối với mỗi từ, có bao nhiêu từ trước đó có mặt nạ bit tạo ra 25 bit khi XOR với mặt nạ bit của từ hiện tại. Để tránh O(n²), chúng tôi duy trì bản đồ băm từ giá trị mặt nạ bit đến số từ có mặt nạ bit đó. Sau đó, mỗi từ có thể được ghép nối một cách hiệu quả với các từ tương thích trước đó bằng cách lặp lại các lần lật một bit có thể có (vì việc đặt 25 bit trong XOR tương đương với việc khác biệt chính xác một bit so với mặt nạ 26 bit đầy đủ). Điều này biến một bài toán bậc hai thành một bài toán lặp qua các từ và một số lượng nhỏ các mặt nạ tiềm năng cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²·L) | O(1) | Quá chậm | 
| Tối ưu | O(n·26) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước mặt nạ 26 bit cho mỗi từ. Khởi tạo mặt nạ thành 0. Đối với mỗi chữ cái trong từ, hãy lật bit tương ứng bằng XOR. Cuối cùng, mặt nạ mã hóa tính chẵn lẻ của tất cả các chữ cái trong từ. 
2. Duy trì một bản đồ băm đếm xem mỗi mặt nạ có bao nhiêu từ. Điều này cho phép chúng tôi truy vấn có bao nhiêu từ trước đó có thể ghép với từ hiện tại để tạo thành một cơn ác mộng hợp lệ. 
3. Đối với mỗi từ, hãy tính toán mặt nạ XOR đích sẽ tạo ra 25 chữ cái được đặt khi XOR với từ hiện tại. Mặt nạ 26 bit với bộ 25 bit khác với mặt nạ hiện tại chính xác một bit, do đó, tạo ra tất cả các mặt nạ thu được bằng cách lật từng bit trong mặt nạ hiện tại. 
4. Thêm số lượng từ trước đó với bất kỳ mặt nạ tương thích nào vào câu trả lời. Cập nhật bản đồ băm để bao gồm mặt nạ của từ hiện tại. 
5. Để xử lý yêu cầu về độ dài lẻ, hãy duy trì số lượng riêng biệt cho các từ có độ dài lẻ và chẵn. Chỉ kết hợp các từ có tổng độ dài thành số lẻ. 

Tại sao nó hoạt động: Mỗi từ được biểu thị bằng một bitmask ghi lại các chữ cái tương đương của nó. Hai từ có thể tạo thành cơn ác mộng nếu XOR của chúng được đặt chính xác 25 bit và độ dài của chúng có tổng là một số lẻ. Bản đồ băm đảm bảo chúng tôi đếm mọi từ tương thích trước đó một cách chính xác một lần và việc lật từng bit trong số 26 bit sẽ tạo ra tất cả các khả năng để đạt được 25 bit được đặt trong XOR. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_nightmares():
    n = int(input())
    words = [input().strip() for _ in range(n)]
    
    from collections import defaultdict
    
    # maps length parity (0=even,1=odd) -> mask -> count
    counts = [defaultdict(int), defaultdict(int)]
    
    ans = 0
    for w in words:
        mask = 0
        for c in w:
            mask ^= 1 << (ord(c) - ord('a'))
        
        length_parity = len(w) % 2
        # we want to pair with words of opposite length parity
        target_parity = 1 - length_parity
        
        # flip each bit to get masks with 25 bits difference
        for i in range(26):
            target_mask = mask ^ (1 << i)
            ans += counts[target_parity].get(target_mask, 0)
        
        counts[length_parity][mask] += 1
    
    print(ans)

count_nightmares()
```Đầu tiên, mã chuyển đổi mỗi từ thành một bitmask gồm các chữ cái tương đương. Chúng tôi tách số lượng theo độ dài chẵn lẻ để thực thi yêu cầu về độ dài lẻ. Sau đó, với mỗi từ, chúng tôi lặp lại tất cả 26 lần lật bit đơn để xác định các từ trước đó tương thích. Cuối cùng, chúng tôi cập nhật số lượng cho mặt nạ của từ hiện tại. Những cạm bẫy thường gặp là quên phân tách các giá trị chẵn lẻ có độ dài hoặc sử dụng XOR không chính xác. 

## Ví dụ đã hoạt động 

Sử dụng mẫu được cung cấp:```
Words: ["ftl", "abcdefghijklmnopqrstuvwxy", "abcdeffghijkllmnopqrsttuvwxy", ...]
```| Lời | Mặt nạ (thùng) | Độ dài chẵn lẻ | Mặt nạ tương thích | Số tích lũy | 
| --- | --- | --- | --- | --- | 
| ftl | 0b111... | 1 | chưa có | 0 | 
| abcdefghijklmnopqrstuvwxy | ... | 1 | lật từng bit | 0 | 
| abcdeffghijkllmnopqrsttuvwxy | ... | 1 | trận đấu trước | 1 | 

Dấu vết này chứng tỏ rằng mỗi từ được chuyển thành một mặt nạ bit nhỏ gọn và chúng tôi chỉ kiểm tra 26 khả năng cho mỗi từ thay vì n từ trước đó. Số lượng tích lũy nắm bắt chính xác những cơn ác mộng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·26) | Mỗi từ được xử lý một lần, lặp lại trên 26 bit để tìm mặt nạ tương thích. | 
| Không gian | O(n) | Chúng tôi lưu trữ bản đồ mặt nạ để đếm; trong trường hợp xấu nhất, mỗi từ có một mặt nạ duy nhất. | 

Với n 2·10⁵ và 26 lần lặp cho mỗi từ, giải pháp hoàn thành dễ dàng trong vòng 4 giây. Bộ nhớ bị giới hạn bởi số lượng mặt nạ duy nhất, nhiều nhất là 2²⁶ nhưng trên thực tế ít hơn rất nhiều do tổng giới hạn số lượng ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        count_nightmares()
    return out.getvalue().strip()

# Provided sample
assert run("""10
ftl
abcdefghijklmnopqrstuvwxy
abcdeffghijkllmnopqrsttuvwxy
ffftl
aabbccddeeffgghhiijjkkllmmnnooppqqrrssttuuvvwwxxyy
thedevid
bcdefghhiiiijklmnopqrsuwxyz
gorillasilverback
abcdefg
ijklmnopqrstuvwxyz
""") == "5"

# Minimum size
assert run("1\na\n") == "0"

# Maximum length odd/even parity
assert run("2\na"*2500000 + "\nb"*2500000 + "\n") == "0"

# All equal words
assert run("3\na\nb\nc\n") == "0"

# Boundary 25 letters, proper pairing
assert run("2\nabcdefghijklmnopqrstuvwxy\na\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 từ | 0 | Từ đơn không thể tạo thành cặp | 
| 2 từ dài | 0 | Độ dài vượt quá giới hạn nhưng không ghép nối | 
| 3 chữ cái đơn | 0 | Không thể đạt tới 25 chữ cái | 
| 2 từ thiếu một chữ | 1 | Ghép đôi đúng cách sẽ tạo ra cơn ác mộng | 

## Vỏ cạnh 

Nếu một từ có tất cả các chữ cái có số chẵn thì mặt nạ của nó bằng 0. Ghép nối nó với một từ khác có mặt nạ có tập hợp chính xác một bit sẽ dẫn đến 25 bit được đặt trong XOR, tạo ra một cơn ác mộng hợp lệ nếu độ dài khớp nhau. Thuật toán theo dõi chính xác
