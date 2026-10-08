---
title: "CF 104945F - Lập trình-tấm bạt lò xo-athlon!"
description: "Mỗi đội trong cuộc thi này được mô tả bằng tên, số lượng bài toán lập trình đã giải và sáu điểm từ các bài tập bạt lò xo. Kết quả cuối cùng của một đội là tổng điểm duy nhất được hình thành bằng cách kết hợp hai phần độc lập."
date: "2026-06-28T07:09:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104945
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC Southwestern European Regional Contest (SWERC 2023)"
rating: 0
weight: 104945
solve_time_s: 66
verified: false
draft: false
---

[CF 104945F - Lập trình-trampoline-athlon!](https://codeforces.com/problemset/problem/104945/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi đội trong cuộc thi này được mô tả bằng tên, số lượng bài toán lập trình đã giải và sáu điểm từ các bài tập bạt lò xo. Kết quả cuối cùng của một đội là tổng điểm duy nhất được hình thành bằng cách kết hợp hai phần độc lập. 

Phần lập trình rất đơn giản: mỗi bài toán giải được sẽ đóng góp một số điểm cố định nên điểm lập trình chỉ phụ thuộc vào số nguyên$P$. Phần tấm bạt lò xo có cấu trúc chặt chẽ hơn một chút: từ sáu điểm do giám khảo đưa ra$E_1 \dots E_6$, giá trị cao nhất và thấp nhất sẽ bị loại bỏ và bốn giá trị còn lại sẽ được tính tổng. Điểm cuối cùng của đội là tổng của hai thành phần này. 

Nhiệm vụ không phải là mô phỏng cuộc thi mà là xếp hạng các đội theo điểm số cuối cùng của họ và đưa ra những kết quả tốt nhất. Một đội được coi là giành huy chương nếu không thực sự kém hơn hai đội khác, điều này tương đương với việc chọn ra một số đội đứng đầu theo điểm số trong khi tôn trọng tỷ số. 

Kích thước đầu vào cho phép lên tới$10^5$các đội. Điều này ngay lập tức loại trừ mọi thứ bậc hai chẳng hạn như so sánh theo cặp hoặc sắp xếp lặp lại bên trong các vòng lặp. Giải pháp ít nhất phải$O(N \log N)$và lý tưởng là tuyến tính hoặc bị chi phối bởi một loại duy nhất. 

Một điểm tinh tế là yêu cầu xử lý cà vạt. Các đội có tổng điểm bằng nhau phải được sắp xếp theo thứ tự đầu vào ban đầu. Điều này có nghĩa là ngay cả sau khi tính điểm, chúng ta vẫn phải duy trì sự ổn định hoặc mã hóa trật tự một cách rõ ràng. 

Trường hợp một cạnh xuất hiện khi nhiều đội có cùng số điểm. Ví dụ: nếu tất cả các đội có tổng số điểm giống nhau thì tất cả các đội đó đều có hiệu lực ngang nhau ở vị trí đầu tiên và tất cả đều phải được đưa vào. Cách giải thích ngây thơ về “top 1000” hoặc “3 thứ hạng riêng biệt hàng đầu” có thể dễ dàng dẫn đến việc cắt ngắn sai nếu các mối quan hệ không được xử lý cẩn thận. 

Một trường hợp khác là tính nhất quán số học trong việc tính điểm tấm bạt lò xo. Việc triển khai bất cẩn có thể quên bỏ chính xác một mức tối đa và một mức tối thiểu hoặc có thể bỏ sai nhiều cực trị giống nhau khi tồn tại các bản sao. Ví dụ, nếu điểm số là$[10, 10, 9, 1, 1, 1]$, chỉ nên loại bỏ một số 10 và một số 1, không phải tất cả các trường hợp. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu bằng cách tính toán tổng điểm của mỗi đội một cách độc lập. Với mỗi đội, chúng tôi tính tổng$P \cdot 10$và điểm số của tấm bạt lò xo được tính bằng cách sắp xếp rõ ràng sáu số và loại bỏ các số cực trị. Phần này đã là công việc liên tục của mỗi nhóm. 

Sau khi tính điểm tất cả, chúng tôi sắp xếp các đội theo tổng điểm theo thứ tự giảm dần rồi chọn ra những thí sinh cao nhất. Sắp xếp$10^5$các mặt hàng là khả thi, nhưng điều phức tạp thực sự nằm ở số lượng chúng ta phải xuất ra. Tuyên bố quy định rằng tối đa 1000 đội sẽ nhận được huy chương, nhưng quy tắc thực tế dựa trên thứ hạng: một đội đủ điều kiện nếu có nhiều nhất hai đội có số điểm cao hơn nghiêm ngặt. Điều này có nghĩa là chúng tôi có thể cần bao gồm tất cả các đội hòa ở ranh giới giới hạn. 

Do đó, cách tiếp cận tối ưu có cấu trúc giống hệt với phương pháp bạo lực, ngoại trừ việc chúng tôi xác định cẩn thận việc sắp xếp và xử lý cắt bỏ. Khi tất cả các điểm được tính toán, chúng tôi thực hiện một sắp xếp duy nhất bằng cách giảm điểm và tăng chỉ số đầu vào. Sau đó, chúng tôi quét từ trên xuống, theo dõi xem có bao nhiêu điểm cao hơn đã được nhìn thấy. Chúng tôi chỉ dừng lại sau khi vượt quá ngưỡng xếp hạng cho phép, nhưng chúng tôi phải bao gồm tất cả các đội hòa ở điểm giới hạn. 

Điều này chuyển vấn đề thành một bước sắp xếp duy nhất cộng với quét tuyến tính, tránh mọi so sánh lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \log N)$|$O(N)$| Có thể chấp nhận được nhưng cần xử lý dây buộc cẩn thận | 
| Tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính tổng điểm cho mỗi đội, sau đó chọn ra các đội có thứ hạng cao nhất và bảo toàn tỷ số hòa một cách cẩn thận. 

### Các bước 

1. Đọc tất cả các đội, gán cho mỗi đội một chỉ số đầu vào từ 0 đến$N-1$. 

Chỉ mục này là cần thiết để phá vỡ các mối quan hệ một cách xác định theo thứ tự đầu vào. 
2. Đối với mỗi đội, hãy tính điểm tấm bạt lò xo bằng cách sắp xếp sáu giá trị của nó. 

Loại bỏ phần tử nhỏ nhất và lớn nhất, sau đó tính tổng bốn giá trị còn lại. 

Việc sắp xếp là an toàn vì chỉ có sáu phần tử nên đây là thời gian không đổi. 
3. Tính tổng điểm như sau$10 \cdot P + \text{trampoline sum}$. 
4. Lưu trữ các bộ dữ liệu có dạng$(-\text{score}, \text{index}, \text{name})$. 

Điểm phủ định đảm bảo rằng việc sắp xếp tăng dần sẽ tạo ra thứ tự điểm giảm dần. 
5. Sắp xếp tất cả các đội theo thứ tự bộ dữ liệu này. 

Điều này thực thi thứ tự chính theo điểm và thứ tự phụ theo thứ tự đầu vào. 
6. Duyệt qua danh sách đã sắp xếp và chọn các đội đồng thời theo dõi xem có bao nhiêu điểm cao hơn được nhìn thấy. 

Vì danh sách được sắp xếp nên nhóm điểm riêng biệt đầu tiên tương ứng với hạng 1, nhóm tiếp theo là hạng 2, v.v. 
7. Bao gồm các đội cho đến khi có nhiều hơn hai nhóm có điểm số cao hơn xuất hiện. 

Khi dừng lại, hãy đảm bảo rằng tất cả các đội có chung điểm giới hạn đều được tính vào. 

### Tại sao nó hoạt động 

Sắp xếp theo tổng điểm, chia các đội thành các khối liền nhau có số điểm bằng nhau. Trong mỗi khối, thứ tự đầu vào duy trì đầu ra xác định. Điều kiện xếp hạng chỉ phụ thuộc vào số lượng mức điểm cao hơn rõ rệt tồn tại phía trên một đội, có thể được theo dõi trong một lần vượt qua. Bởi vì các điểm bằng nhau là liền kề nhau nên chúng tôi không bao giờ chia nhóm huy chương hợp lệ và ranh giới giới hạn có thể được xử lý bằng cách hoàn thành khối điểm hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def trampoline_score(arr):
    arr.sort()
    return sum(arr[1:5])

def solve():
    n = int(input())
    teams = []

    for i in range(n):
        data = input().split()
        name = data[0]
        p = int(data[1])
        e = list(map(int, data[2:]))

        score = 10 * p + trampoline_score(e)
        teams.append((-score, i, name))

    teams.sort()

    result = []
    last_score = None
    distinct_higher = -1

    for idx, (neg_score, i, name) in enumerate(teams):
        score = -neg_score

        if score != last_score:
            distinct_higher += 1
            last_score = score

        if distinc
```
