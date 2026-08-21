---
title: "CF 104115I - \u0414\u0435\u043b\u0435\u043d\u0438\u0435 \u0441\u0442\u0440\u043e\u043a\u0438"
description: "Chúng ta được cho một chuỗi gồm các chữ cái tiếng Anh viết thường và được hỏi liệu nó có thể được chia thành chính xác k phần không trống liền kề nhau sao cho mỗi phần có cùng số chữ cái phụ âm hay không. Phụ âm ở đây có nghĩa là bất kỳ chữ cái nào ngoại trừ a, e, i, o, u, y."
date: "2026-07-02T01:57:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104115
codeforces_index: "I"
codeforces_contest_name: "Voronezh State University - Sitronics contest, 2022"
rating: 0
weight: 104115
solve_time_s: 28
verified: true
draft: false
---

[CF 104115I - \u0414\u0435\u043b\u0435\u043d\u0438\u0435 \u0441\u0442\u0440\u043e\u043a\u0438](https://codeforces.com/problemset/problem/104115/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi gồm các chữ cái tiếng Anh viết thường và được hỏi liệu nó có thể được chia thành chính xác k phần không trống liền kề nhau sao cho mỗi phần có cùng số chữ cái phụ âm hay không. Phụ âm ở đây có nghĩa là bất kỳ chữ cái nào ngoại trừ a, e, i, o, u, y. 

Hạn chế chính là việc phân tách phải bằng cách cắt chuỗi thành các đoạn liên tiếp, vì vậy chúng tôi không sắp xếp lại các ký tự. Chúng ta chỉ chọn k vị trí cắt và k chuỗi con thu được đều phải có số lượng phụ âm bằng nhau. 

Kích thước đầu vào có thể lớn tới 200.000 ký tự, do đó, bất kỳ giải pháp nào thử tất cả các phân vùng có thể hoặc kiểm tra tất cả các kết hợp cắt đều ngay lập tức không khả thi. Cách tiếp cận bậc hai hoặc hàm mũ đối với các vị trí được phân chia hoặc các lựa chọn phân đoạn sẽ vượt quá giới hạn thời gian một khoảng lớn, do đó giải pháp phải là tuyến tính hoặc gần tuyến tính trong n. 

Trường hợp cạnh tinh tế đầu tiên xuất hiện khi k lớn hơn n. Ngay cả khi chuỗi chứa nhiều nguyên âm, mọi phân đoạn đều phải không trống, vì vậy chúng ta không thể có nhiều phân đoạn hơn ký tự. Ví dụ: n = 3, k = 4 luôn bị lỗi bất kể chuỗi nào. 

Một trường hợp cạnh quan trọng khác là khi tổng số phụ âm không chia hết cho k. Vì mỗi đoạn phải có số phụ âm như nhau nên nếu tổng số phụ âm là C thì mỗi phần phải có đúng C/k phụ âm. Nếu C không chia hết cho k thì không có phân vùng nào hoạt động được. Ví dụ: nếu chuỗi có 5 phụ âm và k = 2 thì chúng ta không thể chia 5 đều thành hai phần nguyên. 

Cuối cùng, ngay cả khi khả năng chia hết được giữ nguyên, chúng ta phải đảm bảo rằng việc phân đoạn có thể thực hiện được theo cách liền kề. Điều này đòi hỏi rằng khi chúng tôi quét từ trái sang phải và tích lũy các phụ âm, chúng tôi có thể đặt các vết cắt chính xác ở bội số của số lượng phụ âm mục tiêu trên mỗi phân đoạn. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng đặt k − 1 vết cắt trong số n − 1 vị trí có thể và xác minh từng phân vùng. Đối với mỗi phân vùng ứng cử viên, chúng tôi sẽ tính toán lại số lượng phụ âm trong mỗi phân đoạn. Ngay cả với tổng tiền tố, số lượng kết hợp cắt theo thứ tự chọn k vị trí trong số n, trở nên lớn về mặt tổ hợp khi n là 200.000 và k lên tới 200.000. Cách tiếp cận này là hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có phụ âm mới quan trọng chứ không phải thành phần chính xác của chữ cái. Khi đã biết tổng số phụ âm C, mọi phân vùng hợp lệ phải chia C thành k phần nguyên bằng nhau, mỗi phần bằng C/k. Điều này biến vấn đề thành một lần quét tuyến tính duy nhất: chúng tôi đếm các phụ âm và sau đó gán chúng một cách tham lam vào các phân đoạn theo thứ tự. Bất cứ khi nào chúng tôi tích lũy chính xác các phụ âm C / k, chúng tôi kết thúc một phân đoạn và bắt đầu phân đoạn tiếp theo. 

Bản chất tham lam có tác dụng vì thứ tự chuỗi đã cố định. Không có lợi ích gì trong việc trì hoãn việc cắt hoặc di chuyển nó sớm hơn khi đã đạt đến hạn ngạch cần thiết cho một phân đoạn, vì số lượng phụ âm chỉ tăng khi chúng ta di chuyển sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cắt giảm điều tra | Hàm mũ | O(n) | Quá chậm | 
| Tiền tố + quét tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm tổng số phụ âm trong chuỗi bằng cách quét từng ký tự một lần. Nếu số đếm bằng 0, chúng tôi coi đó là trường hợp đặc biệt vì tất cả các phân đoạn vẫn phải có số lượng bằng nhau, điều này buộc mọi phân đoạn phải có phụ âm bằng 0. 
2. Nếu k lớn hơn n, ngay lập tức trả về “Không” vì chúng ta không thể tạo k phân đoạn không trống. 
3. Nếu tổng số phụ âm C bằng 0 thì mọi đoạn đều phải có phụ âm 0, điều này luôn có thể thực hiện được miễn là chúng ta có thể chia chuỗi thành k phần không trống. Vì bất kỳ sự cắt bỏ nào cũng không ảnh hưởng đến sự đẳng thức của phụ âm nên câu trả lời là “Có” khi k ≤ n. 
4. Nếu C không chia hết cho k, trả về “Không” vì không thể phân phối số nguyên bằng nhau. 
5. Tính target = C/k, là số phụ âm cần có trong mỗi đoạn. 
6. Di chuyển chuỗi từ trái sang phải, duy trì bộ đếm các phụ âm trong đoạn hiện tại. 
7. Mỗi lần chúng ta gặp một phụ âm, hãy tăng bộ đếm đang chạy. 
8. Bất cứ khi nào bộ đếm đang chạy đạt đến mục tiêu, chúng tôi đặt lại nó về 0 và về mặt khái niệm là hoàn thành một phân đoạn. Chúng tôi cũng đếm xem chúng tôi đã hoàn thành bao nhiêu phân đoạn. 
9. Sau khi xử lý toàn bộ chuỗi, hãy kiểm tra xem chúng ta đã hình thành chính xác k đoạn và sử dụng đồng đều tất cả các phụ âm hay chưa. Nếu có thì trả về “Có”, nếu không thì trả về “Không”. 

### Tại sao nó hoạt động 

Tính đúng đắn nằm ở cấu trúc đơn điệu của bài toán. Số lượng phụ âm chỉ tăng khi chúng tôi quét chuỗi, do đó, ranh giới phân đoạn bị buộc phải: một phân đoạn phải kết thúc chính xác khi đạt đến hạn ngạch yêu cầu. Bất kỳ sự cắt giảm nào trước đó sẽ không đủ phụ âm cho phân đoạn và bất kỳ sự cắt giảm nào sau đó sẽ vượt quá hạn ngạch. Vì vậy, nếu tồn tại một phân vùng hợp lệ thì nó phải trùng khớp với cấu trúc tham lam này. Điều kiện phân chia đảm bảo rằng hạn ngạch nhất quán trên toàn cầu và quá trình quét đảm bảo tính khả thi cục bộ ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_vowel(c):
    return c in "aeiouy"

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    consonants = 0
    for ch in s:
        if not is_vowel(ch):
            consonants += 1

    if k > n:
        print("No")
        return

    if consonants == 0:
        print("Yes")
        return

    if consonants % k != 0:
        print("No")
        return

    target = consonants // k

    cnt = 0
    segments = 0

    for ch in s:
        if not is_vowel(ch):
            cnt += 1
        if cnt == target:
            segments += 1
            cnt = 0

    print("Yes" if segments == k and cnt == 0 else "No")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc xây dựng tham lam. Việc kiểm tra nguyên âm được tách biệt cho rõ ràng. Điều kiện tinh tế duy nhất là xác minh cuối cùng: chúng tôi đảm bảo rằng chính xác k phân đoạn đầy đủ đã được hình thành và không còn phụ âm nào còn sót lại, điều này cho thấy phân đoạn cuối cùng chưa hoàn chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 2
polnareff
```Phụ âm là p, l, n, r, f, f nên tổng số là 6. Mục tiêu cho mỗi phân đoạn là 3. 

| Bước | Nhân vật | Số phụ âm | Phân khúc được hình thành | 

|
