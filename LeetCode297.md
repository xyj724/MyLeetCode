```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

//bfs
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root)
            return "";
        queue<TreeNode*> mq;
        string ret{""};
        ret += to_string(root->val) + ","; //special root case
        mq.push(root);
        //for every visiting node, we deal with its children 
        while(!mq.empty())
        {
            TreeNode* node = mq.front();
            mq.pop();
            if(node->left)
            {
                mq.push(node->left);
                ret += to_string(node->left->val) + ",";
            }
            else
                ret += "n,";
            if(node->right)
            {
                mq.push(node->right);
                ret += to_string(node->right->val) + ",";
            }
            else
                ret += "n,";
        }
        //cout << ret;
        return ret;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.size() == 0)
            return NULL;
        string delimiter = ",";
        size_t sep_idx = data.find(delimiter, 0);
        string str = data.substr(0, sep_idx);
        queue<TreeNode*> mq;
        TreeNode* root = new TreeNode(stoi(str));
        mq.push(root);
        while(!mq.empty())
        {
            TreeNode* node = mq.front();
            mq.pop();
            //deal with left children
            size_t next_sep = data.find(delimiter, sep_idx + 1);
            str = data.substr(sep_idx + 1, next_sep - sep_idx - 1);
            if(str == "n")
                node->left = NULL;
            else
            {
                node->left = new TreeNode(stoi(str));
                mq.push(node->left);
            }
            sep_idx = next_sep;
            //deal with right children
            next_sep = data.find(delimiter, sep_idx + 1);
            str = data.substr(sep_idx + 1, next_sep - sep_idx - 1);
            if(str == "n")
                node->right = NULL;
            else
            {
                node->right = new TreeNode(stoi(str));
                mq.push(node->right);
            }
            sep_idx = next_sep;
        }
        return root;
    }
};


```
