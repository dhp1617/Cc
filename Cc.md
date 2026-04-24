###ml
x=np.c_[np.ones(X.shape[0]),X]
B = np.linalg.inv(X.T @ X)@X.T@Y



####logstic 



df = pd.read_csv('data.csv')

df.fillna(df.mean(numeric_only=True), inplace=True)

le = LabelEncoder()
for col in df.select_dtypes(include='object'):
    df[col] = le.fit_transform(df[col])

X = df[['X1','X2']].values   
Y = df['Y'].values

X = np.c_[np.ones(len(X)), X]

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

m = len(Y)
theta = np.zeros(X.shape[1])

learning_rate = 0.1
iterations = 1000
cost_history = []

for i in range(iterations):
    
    z = X @ theta
    h = sigmoid(z)
    
    
    gradient = (X.T @ (h - Y)) / m
    
    theta -= learning_rate * gradient
    
    cost = (-Y*np.log(h) - (1-Y)*np.log(1-h)).mean()
    cost_history.append(cost)

print("Theta:", theta)

plt.plot(range(iterations), cost_history)
plt.xlabel("Iterations")
plt.ylabel("Cost")
plt.show()

pred = sigmoid(X @ theta) >= 0.5
accuracy = (pred == Y).mean()
print("Accuracy:", accuracy)

####slp



df = pd.read_csv('data.csv')

df.fillna(df.mean(numeric_only=True), inplace=True)

le = LabelEncoder()
for col in df.select_dtypes(include='object'):
    df[col] = le.fit_transform(df[col])

X = df[['X1','X2']].values   
Y = df['Y'].values           

scaler = StandardScaler()
X = scaler.fit_transform(X)

X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2)

lr = 0.1
iterations = 1000

weights = np.zeros(X_train.shape[1])
bias = 0

errors = []

def activation(z):
    return np.where(z >= 0, 1, 0)

for i in range(iterations):
    
    z = X_train @ weights + bias
    y_pred = activation(z)
    
    error = Y_train - y_pred
    errors.append(np.mean(np.abs(error)))
    
    weights += lr * (X_train.T @ error) / len(Y_train)
    bias += lr * error.mean()



z_test = X_test @ weights + bias
Y_pred = activation(z_test)




#####MLP 

import torch
import torch.nn as nn
import torch.optim as optim

data = load_iris()
X = data.data
Y = data.target

scaler = StandardScaler()
X = scaler.fit_transform(X)

X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=0)

X_train = torch.tensor(X_train, dtype=torch.float32)
X_test  = torch.tensor(X_test, dtype=torch.float32)
Y_train = torch.tensor(Y_train, dtype=torch.long)
Y_test  = torch.tensor(Y_test, dtype=torch.long)

model = nn.Sequential(
    nn.Linear(4, 10),
    nn.ReLU(),
    nn.Linear(10, 3)
)

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.01)

epochs = 100
losses = []

for i in range(epochs):
    optimizer.zero_grad()
    
    outputs = model(X_train)
    loss = criterion(outputs, Y_train)
    
    loss.backward()
    optimizer.step()
    
    losses.append(loss.item())

plt.plot(losses)
plt.xlabel("Epochs")
plt.ylabel("Loss")
plt.show()

with torch.no_grad():
    outputs = model(X_test)
    _, preds = torch.max(outputs, 1)

y_true = Y_test.numpy()
y_pred = preds.numpy()

####aggloro

df = pd.read_csv("customer_clustering.csv")
df.fillna(df.mean(), inplace= True)
sc = StandardScaler()
x= sc.fit_transform(df)

link = linkage(x, method="single", metric="euclidean")



m1 = AgglomerativeClustering(n_clusters=2 , metric="euclidean", linkage="ward")

l = m1.fit_predict(x)

silhouette_score(x, l )




###fcmean



df = pd.read_csv("driverFCM.csv")

df.fillna(df.mean(numeric_only=True), inplace=True)

X = df.select_dtypes(include=np.number).values

scaler = StandardScaler()
X = scaler.fit_transform(X)

plt.scatter(X[:,0], X[:,1])
plt.title("Input Data Distribution")
plt.show()

sil_scores = []
models = []

for k in range(2, 6):
    
    fcm = FCM(n_clusters=k, m=2, max_iter=150)
    fcm.fit(X)
    
    labels = fcm.predict(X)
    
    sil = silhouette_score(X, labels)
    
    sil_scores.append(sil)
    models.append(fcm)


plt.plot(range(2,6), sil_scores)
plt.xlabel("Clusters")
plt.ylabel("Silhouette Score")
plt.show()

best_idx = np.argmax(sil_scores)
best_model = models[best_idx]

best_labels = best_model.predict(X)

plt.scatter(X[:,0], X[:,1], c=best_labels)
plt.title("FCM Clusters")
plt.show()

print("Centroids:\n", best_model.centers)



####som 



df = pd.read_csv("data.csv")

# 2. Missing value imputation
df.fillna(df.mean(numeric_only=True), inplace=True)

# 3. Standardize
X = df.select_dtypes(include=np.number).values

scaler = StandardScaler()
X = scaler.fit_transform(X)

# 5. Visualize 2 features
plt.scatter(X[:,0], X[:,1])
plt.title("Input Data")
plt.show()

sil_scores = []
models = []

for k in range(2, 6):

    som = MiniSom(x=k, y=1,
                  input_len=X.shape[1],
                  sigma=1.0,
                  learning_rate=0.5)

    som.random_weights_init(X)
    som.train_random(X, 100)

    labels = np.array([som.winner(x)[0] for x in X])

    sil = silhouette_score(X, labels)

    sil_scores.append(sil)
    models.append((som, labels))

    print("k =", k, "Silhouette =", sil)

)
print("Centroids:\n", best_som.get_weights())

plt.scatter(centroids[i, 0], centroids[i, 1], marker='X')



###rules

df = pd.read_csv("Shop1.csv")

df = df.values.astype(str).tolist()

te = TransactionEncoder()
df_arr = te.fit(df).transform(df)
df_bin= pd.DataFrame(df_arr, columns=te.columns_)

feq = apriori(df_bin, min_support=0.02, use_colnames=True)


rules = association_rules(feq, metric="confidence", min_threshold=0.5)
rules


##som
som = MiniSom(x=k, y=1,
                  input_len=X.shape[1],
                  sigma=1.0,
                  learning_rate=0.5)

    som.random_weights_init(X)
    som.train_random(X, 100)

    labels = np.array([som.winner(x)[0] for x in X])

    sil = silhouette_score(X, labels)

###random
x= df[["SepalLengthCm","SepalWidthCm","PetalLengthCm"]].values
sc = StandardScaler()
x= sc.fit_transform(x)

le = LabelEncoder()
y = le.fit_transform(df["Species"])

xt ,xtt, yt ,ytt = train_test_split(x,y , test_size=0.2)

ran = RandomForestClassifier(n_estimators=50 , max_depth=3 )
ran.fit(xt, yt)
yred = ran.predict(xtt)

accuracy_score(yred, ytt)


ad = AdaBoostClassifier(n_estimators=50 , learning_rate=0.01, )
ad.fit(xt , yt)
confusion_matrix(ad.predict(xtt), ytt)



